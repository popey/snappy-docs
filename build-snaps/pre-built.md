---
layout: base
title: Pre-built apps
---

Shipping pre-built binaries makes getting up and running with your application easy, without limiting the number of Linux distributions you support. Snapcraft can ingest these binaries to create snaps that still work on any major Linux distribution.

## What problems do snaps solve for pre-built applications?

Distributed binaries rarely include a mechanism to self-update, and in some case require specific versions of system libraries not consistently available in every Linux distribution’s repositories. With snapcraft it’s just one command to produce a bundle that works anywhere and can be automatically updated.

Here are some snap advantages that will benefit many projects distributing pre-built binaries:

  * Bundle all the runtime requirements, including the exact versions of system libraries.
  * Simplify installation instructions, regardless of distribution, to `snap install myapp`.
  * Directly control the delivery of automatic application updates.
  * Extremely simple creation of daemons.

## Getting started

By way of an example, let's take a look at how a snap is created for the geekbench app.

### geekbench

Snaps are defined in a single yaml file placed in the root of your project. The geekbench example shows the entire `snapcraft.yaml` for an existing project. We'll break this down.

```yaml
name: geekbench4
version: 4.1.0
summary: Cross-Platform Benchmark
description: |
  Geekbench 4 measures your system's power and tells you whether your
  computer is ready to roar. How strong is your mobile device or desktop
  computer? How will it perform when push comes to crunch? These are the
  questions that Geekbench can answer.

confinement: devmode

parts:
  geekbench4:
    plugin: dump
    source: http://cdn.primatelabs.com/Geekbench-$SNAPCRAFT_PROJECT_VERSION-Linux.tar.gz

apps:
  geekbench4:
    command: geekbench4
```

#### Metadata

The `snapcraft.yaml` starts with a small amount of human-readable metadata, which usually can be lifted from the GitHub description or project README.md. This data is used in the presentation of your app in the Snap Store. The `summary:` can not exceed 79 characters. You can use a pipe with the `description:` to declare a multi-line description.

```yaml
name: geekbench4
version: 4.1.0
summary: Cross-Platform Benchmark
description: |
  Geekbench 4 measures your system's power and tells you whether your
  computer is ready to roar. How strong is your mobile device or desktop
  computer? How will it perform when push comes to crunch? These are the
  questions that Geekbench can answer.
```

#### Confinement

To get started we won’t confine this application. Unconfined applications, specified with `devmode`, can only be published to the hidden “edge” channel where you and other developers can install them.

```yaml
confinement: devmode
```


#### Parts

Parts define how to build your app. Parts can be anything: programs, libraries, or other assets needed to create and run your application. In this case we just have one: a tarball containing the geekbench binary. In other cases these can point to local or remote zips files, deb files, or tarballs.

In this example we use the `$SNAPCRAFT_PROJECT_VERSION` variable derived from the `version:` stanza to reference the release tarball.

```yaml
parts:
  geekbench4:
    plugin: dump
    source: http://cdn.primatelabs.com/Geekbench-$SNAPCRAFT_PROJECT_VERSION-Linux.tar.gz
```

#### Apps

Apps are the commands and services exposed to end users. If your command name matches the snap `name`, users will be able run the command directly. If the names differ, then apps are prefixed with the snap `name` (`geekbench4.command-name`, for example). This is to avoid conflicting with apps defined by other installed snaps.

If you don’t want your command prefixed you can request an alias for it on the [Snapcraft forum](https://forum.snapcraft.io/t/process-for-reviewing-aliases-auto-connections-and-track-requests/455). These are set up automatically when your snap is installed from the Snap Store.

```yaml
apps:
  geekbench4:
    command: geekbench4

```

If your application is intended to run as a service you simply add the line `daemon: simple` after the command keyword. This will automatically keep the the service running on install, update and reboot.

### Building the snap

You’ll first need to [install snap support](/core/install), and then install the snapcraft tool:

```
sudo snap install --beta --classic snapcraft
```

If you have just installed snap support, start a new shell so your `PATH` is updated to include `/snap/bin`. You can then build this example yourself:

```
git clone https://github.com/snapcraft-docs/geekbench4
cd geekbench4
snapcraft
```

The resulting snap can be installed locally. This requires the `--dangerous` flag because the snap is not signed by the Snap Store. The `--devmode` flag acknowledges that you are installing an unconfined application:

    sudo snap install geekbench4_*.snap --devmode --dangerous

You can then try running geekbench4.

    geekbench4

Removing the snap is simple too:

    sudo snap remove geekbench4

## Share with your friends

To share your snaps you need to publish them in the Snap Store. First, create an account on [the dashboard](https://dashboard.snapcraft.io/openid/login/?next=/dev/snaps/). Here you can customize how your snaps are presented, review your uploads and control publishing.

You’ll need to choose a unique “developer namespace” as part of the account creation process. This name will be visible by users and associated with your published snaps.

Make sure the `snapcraft` command is authenticated using the email address attached to your Snap Store account:
```
snapcraft login
```

### Reserve a name for your snap

You can publish your own version of a snap, provided you do so under a name you have rights to.

```
snapcraft register mysnap
```
Be sure to update the `name:` in your `snapcraft.yaml` to match this registered name, then run `snapcraft` again.

### Upload your snap

Use snapcraft to push the snap to the Snap Store.

```
snapcraft push --release=edge mysnap_*.snap
```

If you’re happy with the result, you can commit the snapcraft.yaml to your GitHub repo and [turn on automatic builds](https://build.snapcraft.io) so any further commits automatically get released to edge, without requiring you to manually build locally.

## Further customisations

This plugin dumps the content from a specified source. In the cases where dumping the content needs some mangling or organizing one would take advantage of the core functionalities available to plugins such as: `filesets`, `stage`, `prime` and `organize`.

### Extending and overriding behaviour

You can [extend the behaviour](/build-snaps/scriptlets) of any part in your `snapcraft.yaml` with shell commands. These can be run after pulling the source code but before building by using the `prepare` keyword. The build process can be overridden entirely using the `build` keyword and shell commands. The `install` keyword is used to run shell commands after building your code, useful for making post build modifications such as relocating build assets.

<!--
## Next steps

Congratulations, you have an app in edge ready to share with other developers.

Want to learn more? Continue on to learn how to get your app ready for a wider audience.
-->
