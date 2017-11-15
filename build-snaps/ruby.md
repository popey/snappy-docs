---
layout: base
title: Ruby
---

Ruby has rich tools for packaging and distributing applications. Snapcraft builds on top of these familiar tools such as `gem` to create snaps.

## What problems do snaps solve for Ruby applications?

Linux install instructions for Ruby applications often get complicated. To prevent modules from different Ruby applications clashing with each other, developer tools like `rvm` or `rbenv` must be used. With snapcraft it's one command to produce a bundle that works anywhere.

Here are some snap advantages that will benefit many Ruby projects:

  * Bundle all the runtime requirements.
  * Simplify installation instructions, regardless of distribution, to `snap install myrubyapp`.
  * Directly control the delivery of automatic application updates.
  * Extremely simple creation of services.

# Getting started

By way of an example, let's look at how a snap is created for the Travis CI app.

## Travis

Snaps are defined in a single yaml file placed in the root of your project. The Travis example shows the entire `snapcraft.yaml` for an existing project, leveraging the existing `gemspec` to satisfy runtime requirements. We'll break this down.

```yaml
name: travis
version: git
summary: Travis CI CLI Client
description: |
  Command line client interface with a Travis CI service. Works with
  travis-ci.org, travis-ci.com or any custom Travis CI setup you might have.
grade: devel
confinement: devmode

apps:
  travis:
    environment:
      RUBYLIB: $SNAP/usr/lib/ruby/2.3.0:$SNAP/usr/lib/x86_64-linux-gnu/ruby/2.3.0
      GEM_HOME: $SNAP/gems
      GEM_PATH: $SNAP
    command: ruby $SNAP/bin/travis

parts:
  travis.rb:
    source: .
    plugin: nil
    build: gem build travis.gemspec
    install: gem install travis-*.gem -i $SNAPCRAFT_PART_INSTALL
    build-packages: [gcc, libc6-dev, make, ruby-dev]
    stage-packages: [ruby]
```

### Metadata

The `snapcraft.yaml` starts with a small amount of human-readable metadata, which usually can be lifted from the GitHub description or project README.md. This data is used in the presentation of your app in the Snap Store. The `summary:` can not exceed 79 characters. You can use a pipe with the `description:` to declare a multi-line description.

```yaml
name: travis
version: git
summary: Travis CI CLI Client
description: |
  Command line client interface with a Travis CI service. Works with
  travis-ci.org, travis-ci.com or any custom Travis CI setup you might have.
```

### Confinement

To get started we won’t confine this application. Unconfined applications, specified with `devmode`, can only be released to the hidden “edge” channel where you and other developers can install them.

```yaml
confinement: devmode
```

### Parts

Parts define how to build your app. Parts can be anything: programs, libraries, or other assets needed to create and run your application. In this case we have one: the Travis source code. In other cases these can point to local directories, remote git repositories, or tarballs.

This example will also bundle Ruby in the snap, so you can be sure that the version of Ruby you test against is included with your app. Dependencies from your gemspec will also be bundled.

```yaml
parts:
  travis.rb:
    source: .
    plugin: nil
    build: gem build travis.gemspec
    install: gem install travis-*.gem -i $SNAPCRAFT_PART_INSTALL
    build-packages: [gcc, libc6-dev, make, ruby-dev]
    stage-packages: [ruby]
```

### Apps

Apps are the commands and services exposed to end users. If your command name matches the snap `name`, users will be able run the command directly. If they differ, then apps are prefixed with the snap `name` (`travis.command-name`, for example). This is to avoid conflicting with the apps defined by other installed snaps.

If you don’t want your command prefixed you can request an alias for it on the [Snapcraft forum](https://forum.snapcraft.io/t/process-for-reviewing-aliases-auto-connections-and-track-requests/455). These are set up automatically when your snap is installed from the Snap Store.

```yaml
apps:
  travis:
    environment:
      RUBYLIB: $SNAP/usr/lib/ruby/2.3.0:$SNAP/usr/lib/x86_64-linux-gnu/ruby/2.3.0
      GEM_HOME: $SNAP/gems
      GEM_PATH: $SNAP
    command: ruby $SNAP/bin/travis
```

If your application is intended to run as a service you simply add the line `daemon: simple` after the command keyword. This will automatically keep the service running on install, update and reboot.

## Building the snap

You’ll first need to [install snap support](/core/install), and then install the snapcraft tool:
```
sudo snap install --candidate --classic snapcraft
```

If you have just installed snap support, start a new shell so your `PATH` is updated to include `/snap/bin`. You can then build this example yourself:

```
git clone https://github.com/snapcraft-docs/travis.rb
cd travis.rb
snapcraft
```

The resulting snap can be installed locally. This requires the `--dangerous` flag because the snap is not signed by the Snap Store. The `--devmode` flag acknowledges that you are installing an unconfined application:

```
sudo snap install travis_*.snap --devmode --dangerous
```

You can then try running Travis.

```
travis
```

Removing the snap is simple too:

```
sudo snap remove travis
```

## Share with your friends

To share your snaps you need to publish them in the Snap Store. First, create an account on [the dashboard](https://dashboard.snapcraft.io/openid/login/?next=/dev/snaps/). Here you can customize how your snaps are presented, review your uploads and control publishing.

You’ll need to choose a unique “developer namespace” as part of the account creation process. This name will be visible by users and associated with your published snaps.

Make sure the `snapcraft` command is authenticated using the email address attached to your store account:

```
snapcraft login
```

### Reserve a name for your snap

You can publish your own version of a snap, provided you do so under a name you have rights to.

```
snapcraft register myrubysnap
```

Be sure to update the `name:` field in your `snapcraft.yaml` to match this registered name, then run `snapcraft` again.

### Upload your snap

Use snapcraft to push the snap to the Snap Store.

```
snapcraft push --release=edge myrubysnap_amd64.snap
```

If you’re happy with the result, you can commit the snapcraft.yaml to your GitHub repo and [turn on automatic builds](https://build.snapcraft.io) so any further commits automatically get released to edge, without requiring you to manually build locally.

<!--
## Next steps

Congratulations, you have an app in edge ready to share with other developers.

What to learn more? Continue on to learn how to get your app ready for a wider audience.
-->
