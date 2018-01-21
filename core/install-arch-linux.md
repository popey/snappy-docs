---
layout: base
title: Install snapd on Arch Linux
---

snapd is available in the AUR (Arch User Repository) of Arch Linux. It can
be installed via your favorite AUR helper (e.g. yaourt).

```
sudo yaourt -S snapd
```

Once installed the systemd unit which is responsible to manage the
main communication socket for snapd is not automatically enabled and
you have to do this manually:

```
sudo systemctl enable --now snapd.socket
```

Restart the computer for post-install changes to take effect.

Afterwards everything is setup to get you started with snaps.

## Next Steps

 * [Using snaps](usage)
