---
layout: base
title: Install snapd on Manjaro
---

snapd packages for Manjaro are available from the official distribution repositories.

Ensure your packages are up to date and install the snapd package with:

```
sudo pacman -Sy snapd
```

Once the snapd package is successfully installed you have to enable the systemd unit which takes care of snapd's main communication socket as this is not automatically done:

```
sudo systemctl enable --now snapd.socket
```

Now everything is set up to get you started with snaps.

## Next Steps

 * [Using snaps](usage)
