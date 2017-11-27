---
layout: base
title: Rust
---

Rust has rich tools for packaging and distributing applications. Snapcraft builds on top of these familiar tools such as Rustup, Cargo and `rustc` to create snaps.

## What problems do snaps solve for Rust applications?

Linux installation instructions for Rust applications often `curl` a script
and pipe it to a shell. This can be a cause for concern for users and
the installation scripts themselves are complex, have to accommodate
multiple Linux distributions and architectures. With snapcraft it's one
command to produce a bundle that works anywhere.

Here are some snap advantages that will benefit many Rust projects:

  * Bundle all the runtime requirements, with support for multiple architectures.
  * Simplify installation instructions, regardless of distribution, to `snap install myrustapp`.
  * Directly control the delivery of automatic application updates.
  * Extremely simple creation of services.
  * No complex installation shell scripts to maintain.

# Getting started

By way of an example, let's look at how a snap is created for the Parity app.

## Parity

Snaps are defined in a single yaml file placed in the root of your project. The Parity example shows the entire `snapcraft.yaml` for an existing project, leveraging the existing `Cargo.toml` to satisfy runtime requirements. We'll break this down.

```yaml
name: parity
version: git
summary: Fast, light, robust Ethereum implementation
description: |
  Parity's goal is to be the fastest, lightest, and most secure Ethereum
  client. We are developing Parity using the sophisticated and cutting-edge
  Rust programming language. Parity is licensed under the GPLv3, and can be
  used for all your Ethereum needs.

confinement: devmode

apps:
  parity:
    command: parity

parts:
  parity:
    source: .
    plugin: rust
    build-attributes: [no-system-libraries]
    build-packages:
      - libudev-dev
      - libssl-dev
      - make
      - pkg-config
    stage-packages:
      - libssl1.0.0
      - libudev1
      - libstdc++6
```

### Metadata

The `snapcraft.yaml` starts with a small amount of human-readable metadata, which usually can be lifted from the GitHub description or project README.md. This data is used in the presentation of your app in the Snap Store. The `summary:` can not exceed 79 characters. You can use a pipe with the `description:` to declare a multi-line description.

```yaml
name: parity
version: git
summary: Fast, light, robust Ethereum implementation
description: |
  Parity's goal is to be the fastest, lightest, and most secure Ethereum
  client. We are developing Parity using the sophisticated and cutting-edge
  Rust programming language. Parity is licensed under the GPLv3, and can be
  used for all your Ethereum needs.
```

### Confinement

To get started we won't confine this application. Unconfined applications, specified with `devmode`, can only be released to the hidden "edge" channel where you and other developers can install them.

```yaml
confinement: devmode
```

### Parts

Parts define how to build your app. Parts can be anything: programs, libraries, or other assets needed to create and run your application. In this case we have one: the Parity source code. In other cases these can point to local directories, remote git repositories, or tarballs.

This example will also bundle the current stable release of Rust in the snap using Rustup and you can define the exact version of Rust with the optional ` - rust-revision:` keyword, should you have specific requirements. Dependencies from your `Cargo.toml` will also be bundled.
`build-packages:` include any packages from the Ubuntu archive required for just the build step and `stage-packages:` bundles packages from the Ubuntu archive that are required at runtime.

```yaml
parts:
  parity:
    source: .
    plugin: rust
    build-attributes: [no-system-libraries]
    build-packages:
      - libudev-dev
      - libssl-dev
      - make
      - pkg-config
    stage-packages:
      - libssl1.0.0
      - libudev1
      - libstdc++6
```

### Apps

Apps are the commands and services exposed to end users. If your command name matches the snap `name`, users will be able execute the command directly. If they differ, then apps are prefixed with the snap `name` (`parity.command-name`, for example). This is to avoid conflicting with the apps defined by other installed snaps.

If you don't want your command prefixed you can request an alias for it on the [Snapcraft forum](https://forum.snapcraft.io/t/process-for-reviewing-aliases-auto-connections-and-track-requests/455). These are set up automatically when your snap is installed from the Snap Store.

```yaml
apps:
  parity:
    command: parity
```

If your application is intended to run as a service you simply add the line `daemon: simple` after the command keyword. This will automatically keep the service running on install, update and reboot.

## Building the snap

You'll first need to [install snap support](/core/install), and then install the snapcraft tool:
```
sudo snap install --candidate --classic snapcraft
```

If you have just installed snap support, start a new shell so your `PATH` is updated to include `/snap/bin`. You can then build this example yourself:

```
git clone https://github.com/snapcraft-docs/parity
cd parity
snapcraft
```

The resulting snap can be installed locally. This requires the `--dangerous` flag because the snap is not signed by the Snap Store. The `--devmode` flag acknowledges that you are installing an unconfined application:

```
sudo snap install parity_*.snap --devmode --dangerous
```

You can then try running Parity.

```
parity
```

Removing the snap is simple too:

```
sudo snap remove parity
```

## Share with your friends

To share your snaps you need to publish them in the Snap Store. First, create an account on [the dashboard](https://dashboard.snapcraft.io/openid/login/?next=/dev/snaps/). Here you can customize how your snaps are presented, review your uploads and control publishing.

You'll need to choose a unique "developer namespace" as part of the account creation process. This name will be visible by users and associated with your published snaps.

Make sure the `snapcraft` command is authenticated using the email address attached to your store account:

```
snapcraft login
```

### Reserve a name for your snap

You can publish your own version of a snap, provided you do so under a name you have rights to.

```
snapcraft register myrustsnap
```

Be sure to update the `name:` field in your `snapcraft.yaml` to match this registered name, then run `snapcraft` again.

### Upload your snap

Use snapcraft to push the snap to the Snap Store.

```
snapcraft push --release=edge myrustsnap_amd64.snap
```

If you're happy with the result, you can commit the snapcraft.yaml to your GitHub repo and [turn on automatic builds](https://build.snapcraft.io) so any further commits automatically get released to edge, without requiring you to manually build locally.

<!--
## Next steps

Congratulations, you have an app in edge ready to share with other developers.

What to learn more? Continue on to learn how to get your app ready for a wider audience.
-->
