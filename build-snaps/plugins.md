---
layout: base
title: Plugins
---

Plugins are pieces of Python code that tell Snapcraft how to build the content of a snap's parts. There are a variety of plugins already included with snapcraft. You can find the current list from `$ snapcraft list-plugins`, and you should find plugins covering Python 2 and 3, Go, Java, cmake- and autotools-based projects, and more. These supplied plugins will usually be adequate for building most snaps.

See the [Plugins reference](/reference/plugins) for the full list of implemented ones.

## Lifecycle

Snapcraft plugins implement a lifecycle over the following steps:

- `pull`: retrieve the source for the part from the specified location
- `build`: drive the build system determined by the choice of plugin
- `stage`: consolidate desireable files from all the parts in one tree
- `prime`: distill down to only the files which will go into the snap
- `snap`: compress the prime tree into the installable snap file

These steps correspond to snapcraft commands. So when you initiate a `snapcraft pull` you will invoke the respective plugin for each part in the snap, in sequence, to handle the source pull. Each part will then have a fully populated `parts/<part-name/>/src/` directory. Similarly, if you then say `snapcraft build` you will invoke the plugin responsible for each part in turn, to build the part.

## Adding custom plugins

Snapcraft also allows you to add your own plugins written in Python.

You can find a lot of custom plugins in [this repository of examples](https://github.com/ubuntu/snappy-playpen) contributed by the community (such as [Intellij Idea](https://github.com/ubuntu/snappy-playpen/tree/master/idea), [Kylin icon theme](https://github.com/ubuntu/snappy-playpen/tree/master/ubuntukylin-icon-theme), [MirageOS](https://github.com/ubuntu/snappy-playpen/tree/master/mirageos)).

To illustrate how a plugin is created, let's consider adding support for a custom build-tool named "Crafty". The goal of this plugin is to pass a configurable target to the `crafty sometarget` command inside the `src/crafty` source directory.

Relative to the directory of `snapcraft.yaml`, create `snap/plugins/x-crafty.yaml` containing:

```yaml
      options:
          source:
              required: true
          source-type:
          source-tag:
          source-branch:
          crafty-target:
              required: true
```

and `snap/plugins/x_crafty.py` containing:

```python
    import snapcraft

    class XCraftyPlugin(snapcraft.BasePlugin):
        def build(self):
            return self.run([‘crafty’, self.options.crafty_target])

        def pull(self):
            return self.handle_source_options()
```

Note that to avoid collisions with official part plugins, the names are prefixed with "x": `x_crafty`, and `x-crafty`.

This part plugin is then used in `snapcraft.yaml` with:

```yaml
    parts:
      crafted-bits:
        plugin: x-crafty
        source: src/crafty
        crafty-target: sometarget
```

New part plugins are placed in the snap's source directory along `snapcraft.yaml` as they mature, then submitted as patches to the Snapcraft project for everyone to use.

## Examples

If you would like to explore plugins further, here are a couple of plugin examples and basic building details:

### autotools, make: the standard "configure, make, make install" plugins

[See them used](https://github.com/ubuntu-core/snapcraft/blob/master/demos/libpipeline/snap/snapcraft.yaml).

Get more info from:

```bash
    $ snapcraft help autotools
    $ snapcraft help make
```

### go: the Golang plugin

[See it used](https://github.com/ubuntu-core/snapcraft/blob/master/demos/godd/snap/snapcraft.yaml).

Get more info from:

```bash
$ snapcraft help go
```