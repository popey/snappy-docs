---
layout: base
title: Register your snap
---

You can push your own version of a snap, provided you do so under a name you have rights to.

New snaps can be registered by:

* clicking **Register Snap** at the top of the [Snap Store dashboard](https://dashboard.snapcraft.io)
* visiting: [the snap registration page](https://dashboard.snapcraft.io/dev/snaps/register-snap/)
* or running `snapcraft register <snap name>`

## Example

```
snapcraft register drone-autopilot
```

If the name is available, snapcraft will then confirm the registration went through:

```
Registering drone-autopilot.
Congratulations! You're now the publisher for 'drone-autopilot'.
```

You are now the only developer able to use this name in the Snap Store. Note that the Snap Store allows you to share snaps management (push and release) with other developers on a per-snap basis.

## Name disputes

If needed, snaps can be renamed to ensure they match the expectations of most users. If you're the developer or publisher most users expect for a snap name, you may claim it with the [snap registration](https://dashboard.snapcraft.io/dev/snaps/register-name/) form.

## What comes next?

[Uploading your snap](upload)
