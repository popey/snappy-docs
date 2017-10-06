---
layout: base
title: The waf plugin
---

The WAF plugin is useful to build waf based parts

waf bases projects are projects that drive configuration and build via
a local waf python helper - see https://github.com/waf-project/waf for more
details.

### Plugin-specific keywords

    - configflags:
      (list of strings)
      configure flags to pass to the build such as those shown by running
      ./waf --help

[Examples on GitHub](https://github.com/search?o=desc&q=filename%3Asnapcraft.yaml+%22plugin%3A+waf%22+&s=indexed&type=Code&utf8=%E2%9C%93)