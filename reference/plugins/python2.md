---
layout: base
title: The python2 plugin (deprecated)
---

The python2 plugin can be used for python 2 based parts.

This plugin is DEPRECATED in favor of the python plugin.

The python2 plugin can be used for python 2 projects where you would
want to do:

    - import python modules with a requirements.txt
    - build a python project that has a setup.py
    - install sources straight from pip

### Plugin-specific keywords

    - requirements:
      (string)
      Path to a requirements.txt file
    - constraints:
      (string)
      Path to a constraints file
    - process-dependency-links:
      (bool; default: false)
      Enable the processing of dependency links.
    - python-packages:
      (list)
      A list of dependencies to get from PyPi

[Examples on GitHub](https://github.com/search?o=desc&q=filename%3Asnapcraft.yaml+%22plugin%3A+python2%22+&s=indexed&type=Code&utf8=%E2%9C%93)