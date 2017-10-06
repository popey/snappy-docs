---
layout: base
title: Install snapd on Solus
---

snapd packages for Solus are available from the official
distribution repositories.

## Solus 3 (and above)

Solus includes snapd by default starting with the Solus 3 release. No installation steps are required and you can use snapd directly.

### For older versions

Ensure your packages are up to date and install the snapd package with:

```
sudo eopkg up
sudo eopkg install snapd
```

Then reboot to ensure AppArmor and snapd are initialized:

```
sudo reboot
```

Now everything is set up to get you started with snaps.

## Next Steps

 * [Using snaps](usage)
