---
layout: base
title: Scriptlets
---

Scriptlets are shell scripts sourced directly from your `snapcraft.yaml`, to change the behaviour of a plugin.

## Overview

 * Scriptlets can be triggered before [the "build" step of a plugin](/build-snaps/plugins), instead of it and after.

 * They are declared inside parts, using the following keywords: `prepare`, `build` and `install`.

  * Their working directory is the base build directory of the part (`/parts/<part name>/build`) and they are executed with `/bin/sh`.

### Syntax

Scriptlets are declared with the following syntax:

    parts:
      <part name>:
        <scriptlet keyword>: <shell script>

You can use a pipe on the first line to declare a multi-line script:

    parts:
      <part name>:
        <scriptlet keyword>: |
          <multi-line>
          <shell script>

## "prepare"

The `prepare` scriptlet is triggered between the pull and the build step of a plugin. It allows you to run commands on the source of a part before building it.

### Example

#### snapcraft.yaml

```
name: test
version: 1
summary: test
description: test

parts:
  foo:
    source: .
    plugin: dump
    prepare: |
      echo "test" > testfile
      cp testfile testfile2
```

When you run `snapcraft` on this example, you can see that the final snap (content of the `prime` directory) contains the `testfile` and `testfile2` files with our "test" string inside.

Here is the order of operations, the plugin:

 * installs all the required dependencies
 * pulls a local `source` ("`.`" means all the content of the project directory)
 * runs the `prepare` script:

       echo "test" > testfile
       cp testfile testfile2

 * Builds the part using the plugin's build system

## "build"

The `build` scriptlet replaces the build step of a plugin.

In some cases, default plugins don't offer enough flexibility to build your application (specific env variables, custom build systems...), the `build` scriptlet gives you complete freedom to override the plugin's build behaviour.

### Example

#### snapcraft.yaml

```
name: test
version: 1
summary: test
description: test

parts:
  foo:
    source: .
    plugin: python
    python-version: "python3"
    build: |
      cd scripts
      python3 build_dev.py
```

Here is the order of operations, the plugin:

 * installs all the required dependencies
 * pulls a local `source` ("`.`" means all the content of the project directory)
 * runs the `build` script:

       cd scripts
       python3 build_dev.py

## "install"

The `install` scriptlet is triggered after the build step of a plugin.

### Example

#### snapcraft.yaml

```
name: test
version: 1
summary: test
description: test

parts:
  foo:
    source: .
    plugin: autotools
    install: |
      sed -i 's|/usr/bin|$SNAP/usr/bin|g' my-bin-artifact.sh
      mv my-bin-artifact.sh $SNAPCRAFT_PART_INSTALL/bin/my-bin-build.sh
```

Here is the order of operations, the plugin:

 * installs all the required dependencies
 * pulls a local `source` ("`.`" means all the content of the project directory)
 * runs the build step (in this case, the autotools plugin will run: `./configure`, `make`, `make install`)
 * runs the `install` script: :

       sed -i 's|/usr/bin|$SNAP/usr/bin|g' my-bin-artifact.sh
       mv my-bin-artifact.sh $SNAPCRAFT_PART_INSTALL/bin/my-bin-build.sh

   `$SNAPCRAFT_PART_INSTALL` being where snapcraft moves the build to when the build step finishes.