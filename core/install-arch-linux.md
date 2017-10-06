---
layout: base
title: Install snapd on Arch Linux
---

snapd is available in the community repository of Arch Linux. It can
be installed via:

```
sudo pacman -S snapd
```

Once installed the systemd unit which is responsible to manage the
main communication socket for snapd is not automatically enabled and
you have to do this manually:

```
sudo systemctl enable --now snapd.socket
```

Afterwards everything is setup to get you started with snaps.

## Next Steps

 * [Using snaps](usage)
