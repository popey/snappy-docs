---
layout: base
title: The dump plugin
---

This plugin just dumps the content from a specified source.

In the cases where dumping the content needs some mangling or organizing
one would take advantage of the core functionalities available to plugins
such as: `filesets`, `stage`, `snap` and `organize`.

See [common keywords](/reference/plugins/common) for more information about these keywords.

## Example

    parts:
      my-part:
        source: local-source/
        plugin: dump
        organize:
          '*png' : images/
          launch.wrapper: usr/bin/launcher
        snap:
          - usr

[More examples on GitHub](https://github.com/search?o=desc&q=filename%3Asnapcraft.yaml+%22plugin%3A+dump%22+&s=indexed&type=Code&utf8=%E2%9C%93)