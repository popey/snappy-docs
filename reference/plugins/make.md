---
layout: base
title: The make plugin
---

The make plugin is useful for building make based parts.

Make based projects are projects that have a Makefile that drives the
build.

This plugin always runs 'make' followed by 'make install', except when
the 'artifacts' keyword is used.

### Plugin-specific keywords

    - artifacts:
      (list)
      Link/copy the given files from the make output to the snap
      installation directory. If specified, the 'make install'
      step will be skipped.

    - makefile:
      (string)
      Use the given file as the makefile.

    - make-parameters:
      (list of strings)
      Pass the given parameters to the make command.

    - make-install-var:
      (string; default: DESTDIR)
      Use this variable to redirect the installation into the snap.

[Examples on GitHub](https://github.com/search?o=desc&q=filename%3Asnapcraft.yaml+%22plugin%3A+make%22+&s=indexed&type=Code&utf8=%E2%9C%93)