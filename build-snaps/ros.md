---
layout: base
title: Robot Operating System (ROS)
---

Snapcraft builds on top of the `catkin` tool, familiar to any ROS developer, to create snaps for people to install on Linux.

## What problems do snaps solve for ROS applications?

ROS itself is distributed via the OSRF's own Debian archive, along with many community-supported tools. It's possible to get your own application into their archive as well, but it requires that the application is open-source. You're also left with the question of how to update ROS and your application on a robotic platform that has already been shipped. With snapcraft it's just one command to bundle a specific ROS version along with your application into a snap that works anywhere and can be automatically updated.

Here are some snap advantages that will benefit many ROS projects:

 * Bundle all the runtime requirements, including the exact version of ROS, system libraries, etc.
 * Expand the distributions supported beyond just Ubuntu.
 * Directly control the delivery of application updates.
 * Extremely simple creation of daemons.

## Getting started

Let's take a look at the talker and listener out of the ROS tutorials, and show how simple that system is to snap.

### ROS Tutorials

Snaps are defined in a single yaml file placed in the root of your project. Here is the entire `snapcraft.yaml` for this project. Let's break it down.

```yaml
name: ros-talker-listener
version: 0.1
summary: ROS Talker/Listener Example
description: |
  This example requires roscore as well as a talker and listener.

grade: devel
confinement: devmode

parts:
  ros-tutorials:
    source: https://github.com/ros/ros_tutorials.git
    source-branch: kinetic-devel
    plugin: catkin
    rosdistro: kinetic
    source-space: roscpp_tutorials/
    catkin-packages: [roscpp_tutorials]

apps:
  run:
    command: roslaunch roscpp_tutorials talker_listener.launch
```

#### Metadata

The `snapcraft.yaml` starts with a small amount of human-readable metadata, which usually can be lifted from the GitHub description or project README.md. This data is used in the presentation of your app in the Snap Store. The `summary:` can not exceed 79 characters. You can use a pipe with the `description:` to declare a multi-line description.

```yaml
name: ros-talker-listener
version: 0.1
summary: ROS Talker/Listener Example
description: |
  This example requires roscore as well as a talker and listener.
```

#### Confinement

To get started we won't confine this application. Unconfined applications, specified with `devmode`, can only be released to the hidden "edge" channel where you and other developers can install them.

```yaml
confinement: devmode
```

#### Parts

Parts define how to build your app. Parts can be anything: programs, libraries, or other assets needed to create and run your application. In this case we have one: ros-tutorials. Parts can point to local directories, remote git repositories, or tarballs.

The Catkin plugin will bundle `roscore` in the snap. It will also use `rosdep` to determine the dependencies of the `catkin-packages` provided, download them from the ROS archive, and unpack them into the snap. Finally, it will build the `catkin-packages` specified.

**Important note:** Most ROS developers run out of the `devel` space. As a result, it's easy to forget the importance of good install rules, i.e. rules for installing every component of the package necessary to run, or every component necessary to use a given library. The Catkin packages you're building must have good install rules, or Snapcraft won't know which components to place into the snap. Make sure you install binaries, libraries, header files, launch files, etc.

```yaml
parts:
  ros-tutorials:
    source: https://github.com/ros/ros_tutorials.git
    source-branch: kinetic-devel
    plugin: catkin
    rosdistro: kinetic
    source-space: roscpp_tutorials/
    catkin-packages: [roscpp_tutorials]
```

#### Apps

Apps are the commands and services exposed to end users. If your command name matches the snap `name`, users will be able run the command directly. If the names differ, then apps are prefixed with the snap `name` (`ros-talker-listener.command-name`, for example). This is to avoid conflicting with apps defined by other installed snaps.

If you don't want your command prefixed you can request an alias for it on the [Snapcraft forum](https://forum.snapcraft.io). These are set up automatically when your snap is installed from the Snap Store.

Here we simply run a launch file that brings up roscore along with the talker and listener.

```yaml
apps:
  run:
    command: roslaunch roscpp_tutorials talker_listener.launch
```

If your application is intended to run as a service you simply add the line `daemon: simple` after the command keyword. This will automatically keep the service running on install, update, and reboot.

### Building the snap

You’ll first need to [install snap support](/core/install), and then install the snapcraft tool:
```
sudo snap install --beta --classic snapcraft
```

If you have just installed snap support, start a new shell so your `PATH` is updated to include `/snap/bin`. You can then build this example yourself:

```
git clone https://github.com/snapcraft-docs/ros-talker-listener
cd ros-talker-listener
snapcraft
```

The resulting snap can be installed locally. This requires the `--dangerous` flag because the snap is not signed by the Snap Store. The `--devmode` flag acknowledges that you are installing an unconfined application:

```
sudo snap install ros-talker-listener_*.snap --devmode --dangerous
```

You can then try it out:

```
$ ros-talker-listener.run 
<snip>

SUMMARY
========

PARAMETERS
 * /rosdistro: kinetic
 * /rosversion: 1.12.7

NODES
  /
    listener (roscpp_tutorials/listener)
    talker (roscpp_tutorials/talker)

auto-starting new master
process[master]: started with pid [919]
ROS_MASTER_URI=http://localhost:11311

setting /run_id to a2132f48-a959-11e7-a19a-346895ed0f23
process[rosout-1]: started with pid [932]
started core service [/rosout]
process[listener-2]: started with pid [935]
process[talker-3]: started with pid [936]
[ INFO] [1507158810.260508402]: hello world 0
[ INFO] [1507158810.360553002]: hello world 1
[ INFO] [1507158810.460584229]: hello world 2
[ INFO] [1507158810.460985451]: I heard: [hello world 2]
[ INFO] [1507158810.560586692]: hello world 3
[ INFO] [1507158810.560894817]: I heard: [hello world 3]
[ INFO] [1507158810.660587011]: hello world 4
```

Removing the snap is simple too:

```
sudo snap remove ros-talker-listener
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
snapcraft register myrossnap
```

Be sure to update the `name:` in your `snapcraft.yaml` to match this registered name, then run `snapcraft` again.

### Upload your snap

Use snapcraft to push the snap to the Snap Store.

```
snapcraft push --release=edge myrossnap_*.snap
```

If you’re happy with the result, you can commit the snapcraft.yaml to your GitHub repo and [turn on automatic builds](https://build.snapcraft.io) so any further commits automatically get released to edge, without requiring you to manually build locally.

## Further customisations

Here are all the Catkin plugin-specific keywords:

```
    - catkin-packages:
      (list of strings)
      List of catkin packages to build.
      
    - source-space:
      (string)
      The source space containing Catkin packages. By default this is 'src'.
      
    - rosdistro:
      (string)
      The ROS distro required by this system. Defaults to 'indigo'.
      
    - include-roscore:
      (boolean)
      Whether or not to include roscore with the part. Defaults to true.
    
    - rosinstall-files:
      (list of strings)
      List of rosinstall files to merge while pulling. Paths are relative to
      the source.
   
    - catkin-cmake-args:
      (list of strings)
      Configure flags to pass onto the cmake invocation from catkin.
  
    - underlay:
      (object)
      Used to inform Snapcraft that this snap isn't standalone, and is actually
      overlaying a workspace from another snap via content sharing. Made up of
      two properties:
    
      - build-path:
        (string)
        Build-time path to existing workspace to underlay the one being built,
        for example '$SNAPCRAFT_STAGE/opt/ros/kinetic'.
   
      - run-path:
        (string)
        Run-time path of the underlay workspace (e.g. a subdirectory of the
        content interface's 'target' attribute.)
```

You can view them locally by running:

```
snapcraft help catkin
```

### Extending and overriding behaviour

You can [extend the behaviour](/build-snaps/scriptlets) of any part in your `snapcraft.yaml` with shell commands. These can be run after pulling the source code but before building by using the `prepare` keyword. The build process can be overridden entirely using the `build` keyword and shell commands. The `install` keyword is used to run shell commands after building your code, useful for making post build modifications such as relocating build assets.

For example, while the ros_tutorials have proper install rules, say you were creating a snap of an upstream ROS application that didn't, and you wanted a launch file out of it. You could make use of the `install` keyword to get around the lack of install rules:

```yaml
parts:
  ros-tutorials:
    source: https://url/to/bad/workspace
    plugin: catkin
    rosdistro: kinetic
    catkin-packages: [no_install_rules]
    install: |
      mkdir -p "$SNAPCRAFT_PART_INSTALL/opt/ros/kinetic/share/no_install_rules"
      cp -r no_install_rules/launch "$SNAPCRAFT_PART_INSTALL/opt/ros/kinetic/share/no_install_rules/"
```

<!--
## Next steps

Congratulations, you have an app in edge ready to share with other developers.

Want to learn more? Continue on to learn how to get your app ready for a wider audience.
-->
