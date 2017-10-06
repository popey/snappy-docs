---
layout: base
title: The python plugin
---

The python plugin can be used for python 2 or 3 based parts.

It can be used for python projects where you would want to do:

    - import python modules with a requirements.txt
    - build a python project that has a setup.py
    - install packages straight from pip

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
    - python-version:
      (string; default: python3)
      The python version to use. Valid options are: python2 and python3

[Examples on GitHub](https://github.com/search?o=desc&q=filename%3Asnapcraft.yaml+%22plugin%3A+python%22+&s=indexed&type=Code&utf8=%E2%9C%93)