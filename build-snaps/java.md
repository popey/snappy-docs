---
layout: base
title: Java
---

Snapcraft combines Java applications with the desired Java Runtime Environment and appropriate dependencies to create snaps for people to install on Linux.

# What problems do snaps solve for Java applications?

Distributing a Java application for Linux and reaching the widest possible audience was complicated. Ensuring the end-user has the correct version of JRE/SDK and has their environment configured correctly are typical additional steps the user must undertake. When a Linux distribution changes the delivered JRE, this can be problematic for applications. Snapcraft ensures the correct JRE is shipped alongside the application at all times.

Here are some snap advantages that will benefit many Java projects:

* Simplify installation instructions, regardless of distribution, to `snap install myjavaapp`.
* Directly control the delivery of automatic application updates.

# Getting started

By way of an example, let’s take a look at how a simple Java application can be snapped using snapcraft.

## FreePlane Snap

Using a few lines of yaml and the snapcraft tool, a Java application, it's dependencies and the correct JRE can be packaged as a snap. We’ll break this down.

```yaml
name: freeplane
version: '1.6.10' 
summary: Application for Mind Mapping, Knowledge and Project Management
description: |
  Freeplane is a free and open source software application that supports
  thinking, sharing information and getting things done at work, in school
  and at home. The core of the software is tools for mind mapping (also
  known as concept mapping or information mapping) and using mapped
  information.

confinement: devmode

apps:
  freeplane:
    command: desktop-launch $SNAP/freeplane-1.6.10/freeplane.sh

parts:
  freeplane:
    after: [desktop-glib-only]
    plugin: gradle
    source: .
    build: |
      export JAVA_HOME="/usr/lib/jvm/java-8-openjdk-amd64"
      gradle release -x test -x createGitTag
    install: |
      unzip DIST/freeplane_bin-*.zip -d $SNAPCRAFT_PART_INSTALL/
    build-packages:
      - unzip
      - openjdk-8-jdk
```


### Metadata

The `snapcraft.yaml` starts with a small amount of human-readable metadata, which usually can be lifted from the GitHub description or project README.md. This data is used in the presentation of your app in the Snap Store. The `summary:` can not exceed 79 characters. You can use a pipe with the `description:` to declare a multi-line description.

```yaml
name: freeplane
version: '1.6.10' 
summary: Application for Mind Mapping, Knowledge and Project Management
description: |
  Freeplane is a free and open source software application that supports
  thinking, sharing information and getting things done at work, in school
  and at home. The core of the software is tools for mind mapping (also
  known as concept mapping or information mapping) and using mapped
  information.
```

### Confinement

To get started we won’t confine this application. Unconfined applications, specified with `devmode`, can only be released to the hidden “edge” channel where you and other developers can install them.

```yaml
confinement: devmode
```

### Parts

Parts define how to build your app. Parts can be anything: programs, libraries, or other assets needed to create and run your application. In this case we have two parts, the FreePlane source, and a remote `desktop-glib-only` helper part. In other cases these can point to local directories, remote git repositories, or tarballs.

The `desktop-glib-only` helper remote part will configure the runtime environment so that the application integrates well with the desktop envrionment. Other remote parts are available, and can be discovered via the `snapcraft search` command.

The gradle plugin can build the application using standard parameters. In this case however we have overridden some gradle parameters (to disable testing, and prevent a new git tag being created), and set an appropriate `JAVA_HOME` in a `build:` script snippet. In the `install:` script snippet we're unpacking the built application into a directory which later gets incorporated into the final snap, defined by the `$SNAPCRAFT_PART_INSTALL` variable.

The build requires an appropriate JDK/JRE and the install step requires the addition of the `unzip` command, so these are specified as `build-packages`.

```yaml
parts:
  freeplane:
    after: [desktop-glib-only]
    plugin: gradle
    source: .
    build: |
      export JAVA_HOME="/usr/lib/jvm/java-8-openjdk-amd64"
      gradle release -x test -x createGitTag
    install: |
      unzip DIST/freeplane_bin-*.zip -d $SNAPCRAFT_PART_INSTALL/
    build-packages:
      - unzip
      - openjdk-8-jdk
```

### Apps

Apps are the commands and services exposed to end users. If your command name matches the snap `name`, users will be able run the command directly. If the names differ, then apps are prefixed with the snap `name` (`freeplane.command-name`, for example). This is to avoid conflicting with apps defined by other installed snaps.

If you don’t want your command prefixed you can request an alias for it on the [Snapcraft forum](https://forum.snapcraft.io/t/process-for-reviewing-aliases-auto-connections-and-track-requests/455). These are set up automatically when your snap is installed from the Snap Store.

We have prefixed the FreePlane launch script with the `desktop-launch` helper script, provided by the `desktop-glib-only` remote part.

```yaml
apps:
  freeplane:
    command: desktop-launch $SNAP/freeplane-1.6.10/freeplane.sh
```

If your application is intended to run as a service you simply add the line `daemon: simple` after the command keyword. This will automatically keep the service running on install, update, and reboot.

## Building the snap

You’ll first need to [install snap support](/core/install), and then install the snapcraft tool:
```
sudo snap install --beta --classic snapcraft
```

If you have just installed snap support, start a new shell so your `PATH` is updated to include `/snap/bin`. You can then build this example yourself:

```
git clone https://github.com/snapcraft-docs/freeplane
cd freeplane
snapcraft
```

The resulting snap can be installed locally. This requires the `--dangerous` flag because the snap is not signed by the Snap Store. The `--devmode` flag acknowledges that you are installing an unconfined application:

```
sudo snap install freeplane_*.snap --devmode --dangerous
```

You can then try it out

```
freeplane
```

Removing the snap is simple too:

```
sudo snap remove freeplane
```


## Share with your friends

To share your snaps you need to publish them in the Snap Store. First, create an account on [dashboard.snapcraft.io](https://dashboard.snapcraft.io). Here you can customize how your snaps are presented, review your uploads and control publishing.

You’ll need to choose a unique “developer namespace” as part of the account creation process. This name will be visible by users and associated with your published snaps.

Make sure the `snapcraft` command is authenticated using the email address attached to your Snap Store account:

```
snapcraft login
```

### Reserve a name for your snap

You can publish your own version of a snap, provided you do so under a name you have registered.

```
snapcraft register myjavasnap
```

Be sure to update the `name:` in your `snapcraft.yaml` to match this registered name, then run `snapcraft` again.

### Upload your snap

Use snapcraft to push the snap to the Snap Store.

```
snapcraft push --release=edge myjavasnap_*.snap
```

If you’re happy with the result, you can commit the snapcraft.yaml to your GitHub repo and [turn on automatic builds](https://build.snapcraft.io) so any further commits automatically get released to edge, without requiring you to manually build locally.


<!--
## Next steps

Congratulations, you have an app in edge ready to share with other developers.

Want to learn more? Continue on to learn how to get your app ready for a wider audience.
-->
