---
layout: base
title: The autotools plugin
---

The autotools plugin is used for autotools based parts.

Autotools based projects are the ones that have the usual
`./configure && make && make install` instruction set.

The plugin tries to build using ./configure first, if it is not there
it will run ./autogen and if autogen is not there it will run autoreconf.

### Plugin-specific keywords

    - configflags:
      (list of strings)
      configure flags to pass to the build such as those shown by running
      './configure --help'
    - install-via:
      (enum, 'destdir' or 'prefix')
      Whether to install via DESTDIR or by using --prefix (default is
      'destdir')

[Examples on GitHub](https://github.com/search?o=desc&q=filename%3Asnapcraft.yaml+%22plugin%3A+autotools%22+&s=indexed&type=Code&utf8=%E2%9C%93)