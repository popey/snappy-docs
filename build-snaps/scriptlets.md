---
layout: base
title: Scriptlets
---

Scriptlets are shell scripts sourced directly from your `snapcraft.yaml`, to change the behaviour of a plugin as it builds its part. Each step of the part's lifecycle (pull, build, stage, and prime) can be customised.

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

## Overriding the `pull` step

This can be done by utilising the `override-pull` scriptlet. Its working directory is the part's source directory in `parts/<part name>/src/`. In order to run the default `pull` step, call `snapcraftctl pull` from within the scriptlet.


### Example

Let's say you want to patch the source code of the part you're pulling:

```
parts:
  foo:
    plugin: dump
    # ...
    override-pull: |
      snapcraftctl pull
      patch -p1 < $SNAPCRAFT_STAGE/my.patch
```


## Overriding the `build` step

This can be done by utilising the `override-build` scriptlet. Its working directory is the part's base build directory in `parts/<part name>/build/`. In order to run the default `build` step, call `snapcraftctl build` from within the scriptlet.


### Example

Let's say the default build/install process ends up installing files with absolute paths in them, which need to be fixed up to look inside the snap:

```
parts:
  foo:
    plugin: dump
    # ...
    override-build: |
      snapcraftctl build
      sed -i 's|/usr/bin|$SNAP/usr/bin|g' $SNAPCRAFT_PART_INSTALL/my-bin-artifact.sh
```


## Overriding the `stage` step

This can be done by utilising the `override-stage` scriptlet. Its working directory is the staging area in `stage/`. In order to run the default `stage` step, call `snapcraftctl stage` from within the scriptlet.


### Example

Let's say you wanted to tweak a file installed by another part:

```
parts:
  foo:
    plugin: dump
    # ...
    after: [other-part]
    override-stage: |
      snapcraftctl stage
      sed -i 's|/usr/bin|$SNAP/usr/bin|g' other/parts/file
```


## Overriding the `prime` step

This can be done by utilising the `override-prime` scriptlet. Its working directory is the primeing area in `prime/`. In order to run the default `prime` step, call `snapcraftctl prime` from within the scriptlet.


### Example

Let's say you wanted to compile gsetting schemas for the entire priming area

```
parts:
  foo:
    plugin: nil
    after: [all, other, parts]
    override-prime: |
      glib-compile-schemas usr/share/glib-2.0/schemas
```
