---
layout: base
title: Install snapd on Fedora
---

snapd packages for Fedora are available from the official
distribution repositories starting with Fedora 24.

You can install the snapd package with:

```
sudo dnf install snapd
```

Snaps using `classic` confinement, such as code editors, also require a symlink from `/var/lib/snapd/snap` to `/snap`:

```
sudo ln -s /var/lib/snapd/snap /snap
```

Now everything is set up to get you started with snaps.

## Next Steps

 * [Using snaps](usage)
