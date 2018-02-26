---
layout: base
title: Robot Operating System v2 (ROS2)
---

Snapcraft builds on top of the `ament` tool, familiar to any ROS2 developer, to create snaps for people to install on Linux.

## What problems do snaps solve for ROS2 applications?

ROS2 is currently in beta, and is only just starting to be placed into a beta slice of the OSRF's own Debian archive. Getting your own application there will be difficult until it stabilizes, and it requires that your application is open-source. You're also left with the question of how to update ROS2 and your application on a robotic platform that has already been shipped. With snapcraft it's just one command to bundle a specific ROS2 version along with your application (proprietary or open-source) into a snap that works anywhere and can be automatically updated.

Here are some snap advantages that will benefit many ROS2 projects:

 * Bundle all the runtime requirements, including the exact version of ROS2, system libraries, etc.
 * Expand the distributions supported beyond just Ubuntu.
 * Directly control the delivery of application updates.
 * Extremely simple creation of daemons.

## Getting started

**Note:** We strongly recommend using an Ubuntu 16.04 host, VM or container for this guide. While it is possible to use newer releases of Ubuntu, or other Linux distributions, this may result in incorrect libraries being pulled into the build.

Let's take a look at a talker and listener out of the ROS2 examples, and show how simple that system is to snap.

### ROS2 Examples

Snaps are defined in a single yaml file placed in the root of your project. The entire `snapcraft.yaml` for the ros2-talker-listener example is shown below. We'll break this down.

```yaml
name: ros2-talker-listener
version: 0.1
summary: ROS2 Talker/Listener Example
description: |
  This example consists of the ROS2 underlay as well as an example talker and
  listener.

grade: devel
confinement: devmode

parts:
  examples:
    source: .
    plugin: ament
    version: release-beta3

apps:
  talker:
    command: ros2 run examples_rclcpp_minimal_publisher publisher_lambda

  listener:
    command: ros2 run examples_rclpy_minimal_subscriber subscriber_lambda
```

#### Metadata

The `snapcraft.yaml` starts with a small amount of human-readable metadata, which usually can be lifted from the GitHub description or project README.md. This data is used in the presentation of your app in the Snap Store. The `summary:` can not exceed 79 characters. You can use a pipe in the `description:` key to declare a multi-line description.

```yaml
name: ros2-talker-listener
version: 0.1
summary: ROS2 Talker/Listener Example
description: |
  This example consists of the ROS2 underlay as well as an example talker and
  listener.
```

#### Confinement

To get started we won't confine this application. Unconfined applications, specified with `devmode`, can only be released to the hidden "edge" channel where you and other developers can install them.

```yaml
confinement: devmode
```

#### Parts

Parts define how to build your app. Parts can be anything: programs, libraries, or other assets needed to create and run your application. In this case we have one: the ros2 examples source code. Parts can point to local directories, remote git repositories, or tarballs.

The Ament plugin will bundle the requested version of the ROS2 underlay in the snap. It will then use that bootstrapped ROS2 to build the provided workspace, and install it into the snap.

```yaml
parts:
  examples:
    source: .
    plugin: ament
    version: release-beta3
```

#### Apps

Apps are the commands and services exposed to end users. If your command name matches the snap `name`, users will be able run the command directly. If the names differ, then apps are prefixed with the snap `name` (`ros2-talker-listener.command-name`, for example). This is to avoid conflicting with apps defined by other installed snaps.

If you don't want your command prefixed you can request an alias for it on the [Snapcraft forum](https://forum.snapcraft.io/t/process-for-reviewing-aliases-auto-connections-and-track-requests/455). These are set up automatically when your snap is installed from the Snap Store.

Since the examples didn't include any launch files, we have two commands here: one to launch the C++-based talker, and one to launch the Python-based listener.

```yaml
apps:
  talker:
    command: ros2 run examples_rclcpp_minimal_publisher publisher_lambda

  listener:
    command: ros2 run examples_rclpy_minimal_subscriber subscriber_lambda
```

If your application is intended to run as a service you simply add the line `daemon: simple` after the command keyword. This will automatically keep the service running on install, update, and reboot.

### Building the snap

You’ll first need to [install snap support](/core/install), and then install the snapcraft tool:
```
sudo snap install snapcraft --classic
```

If you have just installed snap support, start a new shell so your `PATH` is updated to include `/snap/bin`. You can then build this example yourself:

```
git clone https://github.com/snapcraft-docs/ros2-examples
cd ros2-examples
snapcraft
```

The resulting snap can be installed locally. This requires the `--dangerous` flag because the snap is not signed by the Snap Store. The `--devmode` flag acknowledges that you are installing an unconfined application:

```
sudo snap install ros2-talker-listener_*.snap --devmode --dangerous
```

Now open two terminals. In one, run the talker:

```
$ ros2-talker-listener.talker
Publishing: [Hello, world! 0]
Publishing: [Hello, world! 1]
Publishing: [Hello, world! 2]
Publishing: [Hello, world! 3]
Publishing: [Hello, world! 4]
```

And in the other, run the listener:

```
$ ros2-talker-listener.listener
I heard: [Hello, world! 0]
I heard: [Hello, world! 1]
I heard: [Hello, world! 2]
I heard: [Hello, world! 3]
I heard: [Hello, world! 4]
```

Removing the snap is simple too:

```
sudo snap remove ros2-talker-listener
```

## Share with your friends

To share your snaps you need to publish them in the Snap Store. First, create an account on [dashboard.snapcraft.io](https://dashboard.snapcraft.io/dev/account/). Here you can customize how your snaps are presented, review your uploads and control publishing.

You'll need to choose a unique "developer namespace" as part of the account creation process. This name will be visible by users and associated with your published snaps.

Make sure the `snapcraft` command is authenticated using the email address attached to your Snap Store account:

```
snapcraft login
```

### Reserve a name for your snap

You can publish your own version of a snap, provided you do so under a name you have rights to. You can register a name on [dashboard.snapcraft.io](https://dashboard.snapcraft.io/register-snap/), or by running the following command:

```
snapcraft register myros2snap
```

Be sure to update the `name:` in your `snapcraft.yaml` to match this registered name, then run `snapcraft` again.

### Upload your snap

Use snapcraft to push the snap to the Snap Store.

```
snapcraft push --release=edge myros2snap_*.snap
```

If you're happy with the result, you can commit the snapcraft.yaml to your GitHub repo and [turn on automatic builds](https://build.snapcraft.io) so any further commits automatically get released to edge, without requiring you to manually build locally.

## Further customisations

Here are all the Ament plugin-specific keywords:

```
    - version:
      (string)
      The ROS2 version required by this system. This relates to the ros2 tags.
      Defaults to 'release-beta3'
```

You can view them locally by running:

```
snapcraft help ament
```

<!--
## Next steps

Congratulations, you have an app in edge ready to share with other developers.

Want to learn more? Continue on to learn how to get your app ready for a wider audience.
-->
