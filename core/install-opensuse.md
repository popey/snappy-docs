---
layout: base
title: Install snapd on openSUSE
---

For openSUSE, snapd is not yet included in the official distribution but can be
installed from a community repository which is available [here](https://build.opensuse.org/package/show/system:snappy/snapd).

The repository currently supports the 42.2 release and Tumbleweed. The installation
process for both is similar. First we need to add the repository itself.

For 42.2:

```
sudo zypper addrepo http://download.opensuse.org/repositories/system:/snappy/openSUSE_Leap_42.2/ snappy
```
For Tumbleweed:

```
sudo zypper addrepo http://download.opensuse.org/repositories/system:/snappy/openSUSE_Tumbleweed/ snappy
```

You can then install snapd:

```
sudo zypper install snapd
```

Once the snapd package is successfully installed you have to
enable the systemd unit which takes care about snapd's main
communication socket as this is not yet automatically done:

```
sudo systemctl enable --now snapd.socket
```

Afterwards everything is setup to get you started with snaps.

## Next Steps

 * [Using snaps](usage)
