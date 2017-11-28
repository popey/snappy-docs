---
layout: base
title: Publish your snap
---

You can share your snaps with the world by publishing them to the Snap Store. Alternatively, you can publish to a [brand store](/core/store). 

There are three key stages to publishing a snap:
1. [Registering your snap](register-snap)
2. [Uploading your snap](upload)
3. [Releasing your snap](release)

Before you do any of this, you will need to create a snap store account.

## Create a Snap Store account

To release snaps you will need to create an account on [the dashboard](https://dashboard.snapcraft.io/openid/login/?next=/dev/snaps/). Here you can customize how your snaps are presented and review your uploads.

You'll need to choose a unique "developer namespace" as part of the account creation process. This name will be visible by users and associated with your snaps.

Once you've confirmed your account, you're ready to start pushing your snaps to the Snap Store.

Make sure the `snapcraft` and `snap` commands know about you by logging in using the email address attached to your account.


### Example

```
snapcraft login
snap login you@yourdomain.com
```

Note that `logout` commands are available as well.

## What comes next?

[Registering your snap](register-snap)
