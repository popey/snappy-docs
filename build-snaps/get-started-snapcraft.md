---
layout: base
title: Get started with snapcraft
---

Snapcraft is a command line tool used to build snaps. This guide details the recommended steps to get ready to build snaps.

Snaps are built to run against a base snap containing a minimal common runtime environment. Currently the most popular of those is based around Ubuntu 16.04, and is called `core`. In the future there may be other base snaps built from subsequent releases, or other Linux distributions. For now, all snaps should be built to run against the 'Series 16' core.

It's crucial therefore that snaps are built on an Ubuntu 16.04 base machine, VM or container to be fully compatible with the currently available series 16 core. 

It is technically *possible* to build on a newer release of Ubuntu, or on a different Linux distribution. However this will likely result in incompatible libraries being incorporated in the snap causing the snapped application to malfunction.

## Development platform

The following are well-supported options for building snaps using snapcraft.

  * On top of an Ubuntu 16.04.3 (LTS) installation on bare metal
  * Inside ephermeral LXD containers running Ubuntu 16.04.3 on top of another release of Ubuntu 
  * Inside an Ubuntu 16.04.3 (LTS) Virtual Machine such as kvm, VirtualBox, VMWare or Parallels on another platform such as macOS or Windows 10
  * Under Windows SubSystem for Linux (Bash on Windows) running Ubuntu 16.04.3 using the deb of snapcraft

Currently not well supported or recommended options:

  * Running snapcraft to build snaps directly on Ubuntu 16.10 or above, without an Ubuntu 16.04.3 container 
  * Running snapcraft on another Linux distribution without an Ubuntu 16.04.3 container

The best experience is generally to use a clean Ubuntu 16.04.3 LTS system or LXD containers or VM running Ubuntu 16.04.3. In this guide we will setup snapcraft on a host and configure LXD to build snaps cleanly in a container.

## Get snapd

Snapcraft is itself available as a snap. To install snaps you need snapd. snapd is shipped by default on Ubuntu 16.04 and above. For other Linux distributions folow the relevant [install guide](/core/install).

## Get snapcraft

Snapcraft installation is simple once snapd is available:

`snap install snapcraft --classic`

Confirming snapcraft is correctly installed is simple:

```
$ snapcraft --version
snapcraft, version 2.39
```

In a later step we will further confirm snapcraft capable of building snaps on this environment.

## Setup LXD

LXD installation on Ubuntu is quite straightforward:

`snap install lxd`

LXD requires that your user is in the lxd group. 

```
sudo usermod -g lxd ${USER}
newgrp lxd
```

Configuration of the LXD defaults can be done via the `init` option. Typically accepting the default prompts is sufficient to get a working LXD configuration, usable by snapcraft on the same host:

`sudo lxd init`

Test that LXD (and the lxc client) are correctly installed by starting a container:

`lxc launch ubuntu:16.04 test`

## Test a container build

By now you should be all set. We can test that everything is setup correctly with a few commands in a new directory:

```
mkdir test-snapcraft
cd test-snapcraft
snapcraft init
snapcraft cleanbuild
```

If everything has been configured correctly the sample project will be compressed and a new ephemeral LXD container will be created. The compressed project will be copied into the container, and snapcraft will run there to create the snap. When the process completes, the snap will be pulled from the container, and the container destroyed.

```
$ ls -1
my-snap-name_0.1_amd64.snap
my-snap-name_0.1_source.tar.bz2
snap
```

This is the most commonly used method for building snaps on a local workstation. A snap built this way can be installed locally or shared via the [snap store](/build-snaps/publish) for further testing. 

## Next steps

Your system is now setup to build snaps inside a clean Ubuntu 16.04.3 LTS container. From here you can:

  * Build [your first snap](/build-snaps/your-first-snap). 
  * Follow one of our [language guides](/build-snaps/languages)
  * Join us at the [Snapcraft Forum](https://forum.snapcraft.io/)

