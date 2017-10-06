---
layout: base
title: DN3
---

**Assets in `setup/gui` should now be placed in `snap/gui`.**

_introduced in snapcraft 2.26_

Originally snapcraft had the idea of putting all fixed assets inside a `setup`
directory, this was introduced with the capability of adding GUI assets such
as desktop files and package icons. With the introduction of the `snap`
directory, the `setup` directory becomes superfluous and has been deprecated
in favor of putting GUI assets in `snap/gui` instead of `setup/gui`.

To move to the new schema, all you need to do is:

```
mkdir -p snap
mv setup/gui snap/
```

And if empty:

```
rmdir setup
```

and apply those changes to your VCS if you are using one.