---
layout: base
title: The catkin plugin
---

The catkin plugin is useful for building ROS parts.

### Plugin-specific keywords

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

[Examples on GitHub](https://github.com/search?o=desc&q=filename%3Asnapcraft.yaml+%22plugin%3A+catkin%22+&s=indexed&type=Code&utf8=%E2%9C%93)