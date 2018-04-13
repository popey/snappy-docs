---
layout: base
title: DN8
---

**The 'install' keyword has been replaced by 'override-build'**

_introduced in snapcraft 2.41_

The `install` scriptlet was originally introduced as a way to "install" artifacts after a build. However, as part of an effort to add support for Snapcraft to have this for _all_ lifecycle steps (i.e. not just build), a new scriptlet has been added that encompasses this functionality called `override-build`. `override-build` allows you to override the default `build` step with your own logic, from which you can call `snapcraftctl build` to run the default `build` step.

Let's say you currently had an `install` scriptlet that looked like this:

```
install: |
  echo "This runs after build!"
```

To get equivalent functionality with the `override-build` scriptlet, try this:

```
override-build: |
  snapcraftctl build
  echo "This runs after build!"
```
