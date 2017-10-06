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

After that, everything is set up to get you started with snaps.

### Note for Fedora 24 users

Once the snapd package is successfully installed you have to
enable the systemd unit which takes care of snapd's main
communication socket as this is not automatically done:

```
sudo systemctl enable --now snapd.socket
```

Now everything is set up to get you started with snaps.

## Next Steps

 * [Using snaps](usage)
