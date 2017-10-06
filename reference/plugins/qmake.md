---
layout: base
title: The qmake plugin
---

The qmake plugin is useful for building qmake-based parts.

These are projects that are built using .pro files.

### Plugin-specific keywords

    - options:
      (list of strings)
      additional options to pass to the qmake invocation.
    - qt-version:
      (enum, 'qt4' or 'qt5')
      Version of Qt to use with qmake.
    - project-files:
      (list of strings)
      list of .pro files to pass to the qmake invocation.

[Examples on GitHub](https://github.com/search?o=desc&q=filename%3Asnapcraft.yaml+%22plugin%3A+qmake%22+&s=indexed&type=Code&utf8=%E2%9C%93)