---
layout: base
title: Build on a remote LXD
---

Snapcraft can build snaps on local and remote LXD containers. Building remotely allows developers to leverage the resources of another host, saving disk space, CPU usage and battery life on their local development laptop.

Setting up remote building requires configuration on both the server (remote host) and developer laptop. Additional details on this setup can be found at [Stéphane Graber's blog](https://stgraber.org/2016/04/12/lxd-2-0-remote-hosts-and-container-migration-612/).

### Server Setup

#### Install & Configure LXD

First, install LXD:

       sudo add-apt-repository ppa:ubuntu-lxc/lxd-stable
       sudo apt update
       sudo apt install lxd
       sudo lxd init

Once installed, you can either log into a new shell, or run `newgrp lxd`.

Enable remote access

      lxc config set core.https_address [::]:8443
      lxc config set core.trust_password something-secure

### Laptop setup

Install the LXC client, which will be used by snapcraft to communicate with the remote host LXD installation.

      sudo add-apt-repository ppa:ubuntu-lxc/lxd-stable
      sudo apt update
      sudo apt install lxc-client
      
Configure the laptop to be aware of the remote host. Fill in an appropriate name, and the IP address of the remote host.

      lxc remote add servername 1.2.3.4

At this point, enter the password set in the previous step, enaling remote access. 

### Running snapcraft

Once the above configuration is complete, snapcraft can be used to build remotely with the ```snapcraft cleanbuild --remote servername``` command from within your application directory.

Snapcraft will start a new container, copy the contents of the current directory over to the remote host and build there. Once complete, snapcraft will copy the resulting snap back down to the developer laptop and destroy the container.