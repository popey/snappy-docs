---
layout: base
title: Install snapd on Raspian
---

If you installed Raspbian Lite you first need to enable the `contrib` and
`non-free` repositories in `/etc/apt/sources.list.d/raspi.list`.

You can install snapd on a Raspbian distribution via:

```
sudo apt update
sudo apt install snapd
sudo reboot
```

Afterwards everything is setup to get you started with snaps.

## Next Steps

 * [Using snaps](usage)
