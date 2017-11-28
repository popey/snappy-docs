---
layout: base
title: Mission Oriented Operating Suite (MOOS)
---

Snapcraft supports using the CMake build system, familiar to MOOS developers, to create snaps for people to install on Linux.

## What problems do snaps solve for MOOS applications?

Linux install instructions for MOOS applications often get complicated. System dependencies, which differ from distribution to distribution, must be separately installed. There's no standard packaging in MOOS: typically one builds both MOOS (or MOOS-IvP) and the MOOS application from source. That means not only is the initial distribution difficult, but getting updates is an exercise left to the reader. With snapcraft it’s just one command to produce a bundle that works anywhere and can be automatically updated.

Here are some snap advantages that will benefit many MOOS projects:

 * Bundle all the runtime requirements, including the exact version of MOOS/MOOS-IvP, system libraries, etc.
 * Simplify installation instructions, regardless of distribution, to `snap install mymoosapp`.
 * Directly control the delivery of application updates.
 * Extremely simple creation of daemons.

## Getting started

Let's take a look at the ping pong MOOS example application, and show how simple it is to snap.

### MOOS examples

Snaps are defined in a single yaml file placed in the root of your project. Here is the entire `snapcraft.yaml` for this project. Let's break it down.

```yaml
name: moos-ping
version: 0.1
summary: MOOS Ping Example
description: |
  The MOOS ping example requires the MOOSDB as well as the ping/pong
  application.

grade: devel
confinement: devmode

parts:
  moos:
    source: https://github.com/themoos/core-moos/archive/10.0.2.a-release.tar.gz
    plugin: cmake
    configflags:
      - -DCMAKE_INSTALL_PREFIX=/

  moos-examples:
    source: https://github.com/themoos/examples-docs-moos.git
    source-subdir: docs/examples/AppExamples
    source-depth: 1
    plugin: cmake
    after: [moos]
    artifacts:
      - bin/ex1010

apps:
  MOOSDB:
    command: MOOSDB

  ex1010:
    command: ex1010
```

#### Metadata

The `snapcraft.yaml` starts with a small amount of human-readable metadata, which usually can be lifted from the GitHub description or project README.md. This data is used in the presentation of your app in the Snap Store. The `summary:` can not exceed 79 characters. You can use a pipe with the `description:` to declare a multi-line description.

```yaml
name: moos-ping
version: 0.1
summary: MOOS Ping Example
description: |
  The MOOS ping example requires the MOOSDB as well as the ping/pong
  application.
```

#### Confinement

To get started we won't confine this application. Unconfined applications, specified with `devmode`, can only be released to the hidden "edge" channel where you and other developers can install them.

```yaml
confinement: devmode
```

#### Parts

Parts define how to build your app. Parts can be anything: programs, libraries, or other assets needed to create and run your application. In this case we have two: MOOS itself, and the examples. Parts can point to local directories, remote git repositories, or tarballs.

Since MOOS is its own part, we can make sure the version of MOOS exactly corresponds to the version our application requires, and it'll get bundled into the snap just like the rest of the application.

Note that, since MOOS provides headers and libraries required to build the examples, we specify that the examples should build `after: [moos]`. Snapcraft will then ensure the MOOS part is built before building the examples.

Note also that the examples do not provide suitable install rules. As a result, instead of relying on `make install` to do the right thing, we specify the exact `artifacts` we want from the part, and the cmake plugin copies them over instead of running `make install`.

Finally, note that the `examples-docs-moos` repository contains rather large PDFs. As such, pulling its entire history can take some time, so we use `source-depth: 1` to specify that we only want the latest commit, not the entire history.

```yaml
parts:
  moos:
    source: https://github.com/themoos/core-moos/archive/10.0.2.a-release.tar.gz
    plugin: cmake
    configflags:
      - -DCMAKE_INSTALL_PREFIX=/

  moos-examples:
    source: https://github.com/themoos/examples-docs-moos.git
    source-subdir: docs/examples/AppExamples
    source-depth: 1
    plugin: cmake
    after: [moos]
    artifacts:
      - bin/ex1010
```

#### Apps

Apps are the commands and services exposed to end users. If your command name matches the snap `name`, users will be able run the command directly. If the names differ, then apps are prefixed with the snap `name` (`moos-ping.command-name`, for example). This is to avoid conflicting with apps defined by other installed snaps.

If you don't want your command prefixed you can request an alias for it on the [Snapcraft forum](https://forum.snapcraft.io/t/process-for-reviewing-aliases-auto-connections-and-track-requests/455). These are set up automatically when your snap is installed from the Snap Store.

```yaml
apps:
  MOOSDB:
    command: MOOSDB

  ex1010:
    command: ex1010
```

If your application is intended to run as a service you simply add the line `daemon: simple` after the command keyword. This will automatically keep the service running on install, update, and reboot.

### Building the snap

You’ll first need to [install snap support](/core/install), and then install the snapcraft tool:
```
sudo snap install --beta --classic snapcraft
```

If you have just installed snap support, start a new shell so your `PATH` is updated to include `/snap/bin`. You can then build this example yourself:

```
git clone https://github.com/snapcraft-docs/moos-ping
cd moos-ping
snapcraft
```

The resulting snap can be installed locally. This requires the `--dangerous` flag because the snap is not signed by the Snap Store. The `--devmode` flag acknowledges that you are installing an unconfined application:

```
sudo snap install moos-ping_*.snap --devmode --dangerous
```

You can then try it out by running the MOOSDB in one shell (this could also be made a daemon):

```
moos-ping.MOOSDB
```

Run the example in "pong" mode in another shell:

```
moos-ping.ex1010
```

And finally, run the example in "ping" mode in yet another shell:

```
moos-ping.ex1010 --ping
```

Removing the snap is simple too:

```
sudo snap remove moos-ping
```

## Share with your friends

To share your snaps you need to publish them in the Snap Store. First, create an account on [the dashboard](https://dashboard.snapcraft.io/openid/login/?next=/dev/snaps/). Here you can customize how your snaps are presented, review your uploads and control publishing.

You'll need to choose a unique "developer namespace" as part of the account creation process. This name will be visible by users and associated with your published snaps.

Make sure the `snapcraft` command is authenticated using the email address attached to your Snap Store account:

```
snapcraft login
```

### Reserve a name for your snap

You can publish your own version of a snap, provided you do so under a name you have registered.

```
snapcraft register mymoossnap
```

Be sure to update the `name:` in your `snapcraft.yaml` to match this registered name, then run `snapcraft` again.

### Upload your snap

Use snapcraft to push the snap to the Snap Store.

```
snapcraft push --release=edge mymoossnap_*.snap
```

If you’re happy with the result, you can commit the snapcraft.yaml to your GitHub repo and [turn on automatic builds](https://build.snapcraft.io) so any further commits automatically get released to edge, without requiring you to manually build locally.

## Further customisations

Here are all the CMake plugin-specific keywords:

```
    - artifacts:
      (list)
      Link/copy the given files from the make output to the snap
      installation directory. If specified, the 'make install'
      step will be skipped.

    - make-parameters:
      (list of strings)
      Pass the given parameters to the make command.

    - make-install-var:
      (string; default: DESTDIR)
      Use this variable to redirect the installation into the snap.

    - configflags:
      (list of strings)
      configure flags to pass to the build using the common cmake semantics.
```

You can view them locally by running:

```
snapcraft help cmake
```

<!--
## Next steps

Congratulations, you have an app in edge ready to share with other developers.

Want to learn more? Continue on to learn how to get your app ready for a wider audience.
-->
