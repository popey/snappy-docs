---
layout: base
title: The godeps plugin
---

The godeps plugin can be used for godeps-enabled go projects.

These projects have a file containing information about the project's
dependencies. This file is typically called "dependencies.tsv," but may be
named anything.

### Plugin-specific keywords

    - godeps-file:
      (string)
      Path to the godeps dependencies file contained within the source
      (default: dependencies.tsv)

    - go-importpath:
      (string)
      This entry tells the checked out `source` to live within a certain path
      within `GOPATH`. This is required in order to work with absolute imports
      and import path checking.

[Examples on GitHub](https://github.com/search?o=desc&q=filename%3Asnapcraft.yaml+%22plugin%3A+godeps%22+&s=indexed&type=Code&utf8=%E2%9C%93)