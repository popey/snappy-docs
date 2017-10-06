---
layout: base
title: 'snapcraft: command reference'
---

## General commands



#### help

Obtain help for a certain plugin or topic.

Usage:

    $ snapcraft help autotools
    This plugin is useful for building parts that use maven.

    The maven build system is commonly used to build Java projects.
    The plugin requires a pom.xml in the root of the source tree.
    ...



#### init

Initialize a snapcraft project.

  Usage:

    $ snapcraft init
    Created snap/snapcraft.yaml.
    Edit the file to your liking or run `snapcraft` to get started



#### list-plugins

List the available plugins that handle different types of part.

  Usage:

    $ snapcraft list-plugins
    ant        cmake  gulp    kernel  nil      python3  tar-content
    autotools  copy   jdk     make    nodejs   qmake
    catkin     go     kbuild  maven   python2  scons



#### login

Authenticate session against Ubuntu One.

  Usage:

    $ snapcraft login
    Enter your Ubuntu One credentials.
    Email: foo@bar.com
    Password:
    One-time password (just press enter if you don't use two-factor authentication):
    123456
    Authenticating against Ubuntu One.
    Login successful.



#### logout

Clear session credentials.

  Usage:

    $ snapcraft logout
    Clearing credentials for Ubuntu One.
    Credentials cleared.



#### register

Register a package name in the Snap Store.

  Usage:

    $ snapcraft register foo
    Registering foo.
    Congratulations! You're now the publisher for 'foo'.



#### tour

Setup the snapcraft examples tour in the specified directory, or `./snapcraft-tour/`.

  Usage:

    $ snapcraft tour
    Snapcraft tour initialized in ./snapcraft-tour/
    Instructions are in the README, or https://snapcraft.io/create/#begin



#### upload

Upload a snap to the Snap Store.

  Usage:

    $ snapcraft upload foo*.snap
    Uploading foo_0_amd64.snap [================================] 100%



## Lifecycle commands

Calling snapcraft without a COMMAND will default to `snapcraft snap` and run the complete snapping lifecycle (pull -> build -> stage -> prime -> snap).



#### snap

Create a snap, by running the complete lifecycle: pull, build, stage, prime, snap.



#### clean

Remove content - cleans downloads, builds or install artifacts.

When used with  `-s <step>` or `--step <step>`, only cleans the specified step and those that depend upon it. <step> can be one of: pull, build, stage or prime.



#### cleanbuild

Create a snap using a pristine environment managed by lxd.

This command requires a properly setup lxd environment that can connect to external networks. Refer to the "Ubuntu Desktop and Ubuntu Server" section in the [lxd documentation](https://linuxcontainers.org/lxd/getting-started-cli) to get started.



#### pull

Download or retrieve artifacts defined for one or all parts.



#### build

Build artifacts defined for one or all parts. Build systems capable of running parallel build jobs will do so unless `--no-parallel-build` is specified.



#### stage

Stage one or all part's built artifacts into the common staging area.



#### prime

Final copy and preparation for the snap.



## Parts ecosystem commands



#### update

Updates the parts listing from the cloud. This pulls parts from [https://parts.snapcraft.io/v1/parts.yaml](https://parts.snapcraft.io/v1/parts.yaml) and makes them available for your snapcraft project.



#### search

Searches the parts listing for all or matching parts.

Usage:

    $ snapcraft search
    PART NAME                          DESCRIPTION
    ffmpeg                             This sets up ffmpeg for projects.
    desktop/gtk3                       Helpers for gtk2, gtk3, qt4 and qt5 or gli...
    curl                               A tool and a library (usable from many lan...
    mqtt-paho                          mqtt-paho for python.
    [...]



#### define

Shows the description, definition and maintainer for a cloud part.

Usage:

    $ snapcraft define desktop/gtk3
    Maintainer: 'Snapcraft community <snapcraft@lists.snapcraft.io>'
    Description: 'Helpers for gtk2, gtk3, qt4 and qt5 or glib minimal launchers.
    It brings the necessary code and exports for binding and using those
    desktop technologies in a relocatable fashion, enabling binding with
    global desktop theme, icon theme, image caching, fonts, mimetype handlers
    application global menu and gsettings integration.
    It also brings basics ubuntu dependency packages.

    Usage :
      1. add "after: [desktop/<technology>]" to your launcher:
         - gtk2, gtk3, qt4 and qt5 corresponds to their respective toolkit
           main dependencies and default choices.
    [...]



## Flags

    --enable-geoip

Enables geoip for the pull step if stage-packages are used.

    --no-parallel-build

  Use only a single build job per part (the default number of jobs per part is equal to the number of CPUs)