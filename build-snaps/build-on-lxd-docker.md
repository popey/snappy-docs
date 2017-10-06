---
layout: base
title: "Build on LXD or Docker"
---

Snaps can be built inside containers, allowing you to build in a pristine environment or one tailored to your needs.

## LXD

Using LXD, you'll map your home directory (or wherever your projects live) into the container, then build a snap from the current directory.

### On distributions that support running snaps

First, install LXD and snapcraft:

    sudo snap install lxd snapcraft

Then, configure LXD:

    sudo newgrp lxd
    sudo lxd init

The `lxd init` wizard will take you through configuration steps. You can keep the default suggestions.

When this is done, you can use the `snapcraft cleanbuild` command on a snap project, to automatically build the snap inside a LXD container.

### On other distributions

If your distribution is unable to run snaps, you can install LXD through other packages formats [such as DEB, AUR, RPM, etc.](https://linuxcontainers.org/lxd/getting-started-cli/)

Once this is done, configure LXD:

    sudo newgrp lxd
    sudo lxd init

The `lxd init` wizard will take you through configuration steps. You can keep the default suggestions.

Next, create an Ubuntu 16.04 container for building snaps. We'll assume your projects live within your home directory and map it into this container. If your projects live elsewhere, substitute `source=/home/$USER` and `path=/home/ubuntu` below for your project directory.

    lxc launch ubuntu:16.04 snapcraft -c security.privileged=true
    lxc config device add snapcraft homedir disk source=/home/$USER path=/home/ubuntu

Finally, install snapcraft into the container:

    lxc exec snapcraft -- apt install snapcraft

You're all set. Any time you want to build a snap, type the following command to run snapcraft relative to the current directory:

    lxc exec snapcraft -- sh -c "cd $(pwd); snapcraft"

## Docker

Using Docker, you'll map the current directory into the container, then build a snap from that same directory.

First, install Docker using these abridged instructions. A more compherensive guide can be found on the [Docker website](https://docs.docker.com/engine/installation/linux/ubuntulinux/).

    echo "deb https://apt.dockerproject.org/repo ubuntu-trusty main" | sudo tee /etc/apt/sources.list.d/docker.list
    sudo apt-key adv \
        --keyserver hkp://ha.pool.sks-keyservers.net:80 \
        --recv-keys 58118E89F3A912897C070ADBF76221572C52609D
    sudo apt update
    sudo apt install docker-engine

Next, make sure Docker is running:

    sudo service docker status

You're all set. Any time you want to build a snap, type the following commands to run snapcraft relative to the current directory:

    sudo docker pull snapcore/snapcraft
    sudo docker run -v $PWD:$PWD -w $PWD snapcore/snapcraft snapcraft
