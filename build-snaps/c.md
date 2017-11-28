---
layout: base
title: C/C++
---

Snapcraft builds on top of tools like autotools, make, and cmake to create snaps for people to install on Linux.

# What problems do snaps solve for C/C++ applications?

Snapcraft bundles necessary libraries required by the application, and can configure the environment for confinement of applications for end user peace of mind. Developers can ensure their application is delivered pre-packaged with libraries which will not be replaced or superceded by a distribution vendor.

Here are some snap advantages that will benefit many C/C++ projects:

* Simplify installation instructions, regardless of distribution, to snap install myapp.
* Directly control the delivery of automatic application updates.
* Bundle all the runtime requirements, including the exact versions of system libraries.
* Extremely simple creation of services.

# Getting started

By way of an example, let’s take a look at how a C application can be snapped using snapcraft.

## DOSBox Snap

Snaps are defined in a single yaml file placed in the root of your project. The DOSBox example shows the entire snapcraft.yaml for an existing project. We'll break this down.

```yaml
name: dosbox
version: "0.74-svn"
summary: DOSBox
description: |
  DOSBox is a x86 emulator with Tandy/Hercules/CGA/EGA/VGA/SVGA graphics
  sound and DOS. It's been designed to run old DOS games under platforms that
  don't support it.

confinement: devmode

apps:
  dosbox:
    command: dosbox

parts:
  dosbox:
    plugin: autotools
    source-type: tar
    source: http://source.dosbox.com/dosboxsvn.tgz
    build-packages:
      - g++
      - make
      - libsdl1.2-dev
      - libpng12-dev
      - libsdl-net1.2-dev
      - libsdl-sound1.2-dev
      - libasound2-dev

```


### Metadata

The `snapcraft.yaml` starts with a small amount of human-readable metadata, which usually can be lifted from the GitHub description or project README.md. This data is used in the presentation of your app in the Snap Store. The `summary:` can not exceed 79 characters. You can use a pipe with the `description:` to declare a multi-line description.

```yaml
name: dosbox
version: "0.74-svn"
summary: DOSBox
description: |
  DOSBox is a x86 emulator with Tandy/Hercules/CGA/EGA/VGA/SVGA graphics
  sound and DOS. It's been designed to run old DOS games under platforms that
  don't support it.
```

### Confinement

To get started we won’t confine this application. Unconfined applications, specified with `devmode`, can only be released to the hidden “edge” channel where you and other developers can install them.

```yaml
confinement: devmode
```

### Parts

Parts define how to build your app. Parts can be anything: programs, libraries, or other assets needed to create and run your application. In this case we have one: the DOSBox source release tarball. In other cases these can point to local directories, remote git repositories or other revision control systems.

Before building the part the dependencies listed as `build-packages` are installed. The autotools plugin uses the standard tools, `configure` and `make` to build the part. 

```yaml
parts:
  dosbox:
    plugin: autotools
    source-type: tar
    source: http://source.dosbox.com/dosboxsvn.tgz
    build-packages:
      - g++
      - make
      - libsdl1.2-dev
      - libpng12-dev
      - libsdl-net1.2-dev
      - libsdl-sound1.2-dev
      - libasound2-dev
```

### Apps

Apps are the commands and services exposed to end users. If your command name matches the snap `name`, users will be able run the command directly. If the names differ, then apps are prefixed with the snap `name` (`dosbox.command-name`, for example). This is to avoid conflicting with apps defined by other installed snaps.

If you don’t want your command prefixed you can request an alias for it on the [Snapcraft forum](https://forum.snapcraft.io/t/process-for-reviewing-aliases-auto-connections-and-track-requests/455). These are set up automatically when your snap is installed from the Snap Store.

```yaml
apps:
  dosbox:
    command: dosbox
```

If your application is intended to run as a service you simply add the line `daemon: simple` after the command keyword. This will automatically keep the service running on install, update, and reboot.

## Building the snap

You’ll first need to [install snap support](/core/install), and then install the snapcraft tool:
```
sudo snap install --candidate --classic snapcraft
```

If you have just installed snap support, start a new shell so your `PATH` is updated to include `/snap/bin`. You can then build this example yourself:

```
git clone https://github.com/snapcraft-docs/dosbox
cd dosbox
snapcraft
```

The resulting snap can be installed locally. This requires the `--dangerous` flag because the snap is not signed by the Snap Store. The `--devmode` flag acknowledges that you are installing an unconfined application:

```
sudo snap install dosbox_*.snap --devmode --dangerous
```

You can then try it out

```
dosbox
```

Removing the snap is simple too:

```
sudo snap remove dosbox
```


## Share with your friends

To share your snaps you need to publish them in the Snap Store. First, create an account on [dashboard.snapcraft.io](https://dashboard.snapcraft.io/openid/login/?next=/dev/snaps/). Here you can customize how your snaps are presented, review your uploads and control publishing.

You’ll need to choose a unique “developer namespace” as part of the account creation process. This name will be visible by users and associated with your published snaps.

Make sure the `snapcraft` command is authenticated using the email address attached to your Snap Store account:

```
snapcraft login
```

### Reserve a name for your snap

You can publish your own version of a snap, provided you do so under a name you have registered.

```
snapcraft register myapp
```

Be sure to update the `name:` in your `snapcraft.yaml` to match this registered name, then run `snapcraft` again.

### Upload your snap

Use snapcraft to push the snap to the Snap Store.

```
snapcraft push --release=edge myapp_*.snap
```

If you’re happy with the result, you can commit the snapcraft.yaml to your GitHub repo and [turn on automatic builds](https://build.snapcraft.io) so any further commits automatically get released to edge, without requiring you to manually build locally.


<!--
## Next steps

Congratulations, you have an app in edge ready to share with other developers.

Want to learn more? Continue on to learn how to get your app ready for a wider audience.
-->
