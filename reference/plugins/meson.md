---
layout: base
title: The meson plugin
---

The meson plugin is useful for building meson based parts.

Meson based projects are projects that have a meson.build that drives the
build.

This plugin always runs `meson snapbuild` followed by `ninja` and `ninja install`.
### Plugin-specific keywords

    - meson-parameters:
      (list of strings)
      Pass the given parameters to the meson command.


[Examples on GitHub](https://github.com/search?o=desc&q=filename%3Asnapcraft.yaml+%22plugin%3A+meson%22+&s=indexed&type=Code&utf8=%E2%9C%93)