---
layout: base
title: Install snapd on Ubuntu
---

### Ubuntu 16.04 (and derivatives)

Ubuntu includes snapd by default starting with the 16.04 LTS (xenial) release. No installation steps are required and you can use snapd directly.

### Ubuntu 14.04

For the older 14.04 LTS (trusty) release or any flavour (e.g. Lubuntu) which doesn't
include snapd by default, you have to install it manually from the archive:

```
sudo apt update
sudo apt install snapd
```

### Additional steps

#### Missing pulseaudio

Snaps which use the pulseaudio interface to playback sounds & music also require pulseaudio to be installed. This is already installed for the majority of Ubuntu flavours, however Lubuntu does not ship pulseaudio, so it must be installed manually if audio is desired from those snaps.

```
sudo apt install pulseaudio
```
Once installed, logout and back in to ensure pulseaudio is running.

#### Unable to launch URLs from snapped applications

Some snaps require the use of an optional package to launch external browser sessions, such as to open online help, documentation, or to perform web based logins. On Ubuntu 16.04 and above, and on derivatives based on 16.04 and above, install that package from the archive:

```
sudo apt update
sudo apt install snapd-xdg-open
```


Afterwards everything is setup to get you started with snaps.

## Next Steps

 * [Using snaps](usage)
