---
layout: base
title: The kbuild plugin
---

The kbuild plugin is used for building kbuild based projects as snapcraft
parts.

This plugin is based on the snapcraft.BasePlugin and supports the properties
provided by that plus the following kbuild specific options with semantics as
explained above:

    - kdefconfig:
      (list of kdefconfigs)
      defconfig target to use as the base configuration. default: "defconfig"

    - kconfigfile:
      (filepath)
      path to file to use as base configuration. If provided this option wins
      over kdefconfig. default: None

    - kconfigs
      (list of strings)
      explicit list of configs to force; this will override the configs that
      were set as base through kdefconfig and kconfigfile and dependent configs
      will be fixed using the defaults encoded in the kbuild config
      definitions.  If you don't want default for one or more implicit configs
      coming out of these, just add them to this list as well.

The plugin applies your selected defconfig first by running

    make defconfig

and then uses the kconfigs flag to augment the resulting config by prepending
the configured kconfigs values to the .config and running

    "yes" "" | make oldconfig

to create an updated .config file.

If kconfigfile is provided this plugin will use the provided config file
wholesale as the starting point instead of make $kdefconfig. In case user
configures both a kdefconfig as well as kconfigfile, kconfigfile approach will
be used.

[Examples on GitHub](https://github.com/search?o=desc&q=filename%3Asnapcraft.yaml+%22plugin%3A+kbuild%22+&s=indexed&type=Code&utf8=%E2%9C%93)