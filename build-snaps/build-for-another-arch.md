---
layout: base
title: Build for another architecture
---

There are many ways to build a snap to a target CPU architecture (for example `armhf`).

Let's take a look at common methods.

## On Launchpad

[Launchpad](http://launchpad.net) can build a branch (git or bzr) with a `snapcraft.yaml` file to any of the supported architectures.

The basic steps are:

1. Push the branch to a Launchpad project (create a project as needed).
2. Click "Create Snap Package" and fill out the form as appropriate, for example:

     * Select the Ubuntu series to build against and the target CPU architecture to build snaps for
     * Specify a PPA to use for pulling and building dependencies
     * Configure automatic building on branch changes (you can always request a build manually after setup)
     * Configure the auto build to release on success to a specified Snap Store channel

Depending on work loads, there may a short delay. For faster iteration, you can build on your target Ubuntu Core device.

## On an Ubuntu Core device

Building on the device itself is a natural way to build to that device's architecture.

While Ubuntu Core devices are confined and initially lack the ability to install the debian packages needed to build, you can install the classic snap to enable all of this.

So, get a terminal to your Ubuntu Core device, and:

1. Install the classic snap:

       sudo snap install classic --edge --devmode

1. Get a bash terminal into a classic environment:

       sudo classic

1. Install required build and development debian packages as usual with `sudo apt-get install snapcraft`. You might want to install git or bzr, for example, to get your repos locally.

Now, you are ready to run `snapcraft` and build your snap to the Ubuntu Core device's native architecture.

## Other setups

There are other ways too. For example, you can make use a chroot and add qemu to it, one can even use a debian package oriented chroot based build approach such as pbuilder.