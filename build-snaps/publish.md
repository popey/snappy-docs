---
layout: base
title: Publish your snap
---

You can share your snaps with the world by publishing them to the Snap Store. Alternatively, you can publish to a [brand store](/core/store).

## 1. Create a Snap Store account

To release snaps you will need to create an account on [the dashboard](https://dashboard.snapcraft.io/openid/login/?next=/dev/snaps/). Here you can customize how your snaps are presented, review your uploads, and control the release process.

You'll need to choose a unique "developer namespace" as part of the account creation process. This name will be visible by users and associated with your snaps.

Once you've confirmed your account, you're ready to start pushing your snaps to the Snap Store.

Make sure the `snapcraft` and `snap` commands know about you by logging in using the email address attached to your account.


### Example

    $ snapcraft login
    $ snap login you@yourdomain.com

Note that `logout` commands are available as well.

## 2. Reserve a name for your snap

You can push your own version of a snap, provided you do so under a name you have rights to.

New names can be registered:

* by clicking **New Snap** at the top of the Snap Store dashboard
* by visiting: [the name registration page](https://dashboard.snapcraft.io/dev/snaps/register-name/)
* or by running `snapcraft register snap-name`


### Example


    $ snapcraft register drone-autopilot
    Registering drone-autopilot.
    Congratulations! You're now the publisher for 'drone-autopilot'.

You are now the only developer able to use this name in the Snap Store. Note that the Snap Store allows you to share snaps management (push and release) with other developers on a per-snap basis.

### Name disputes

If needed, snaps can be renamed to ensure they match the expectations of most users. If you're the developer or publisher most users expect for a snap name, you may claim it with the [snap name registration](https://dashboard.snapcraft.io/dev/snaps/register-name/) form.

## 3. Upload your snap

Before you upload your snap, have a quick look at your `snapcraft.yaml` file again. Two settings (`grade` and `confinement`) will define which [channels](#release-channels) you can release your snap to.

|                 | `confinement: strict`  | `confinement: classic` | `confinement: devmode` |
| --------------- | ---------------------- | ---------------------- | ---------------------- |
| `grade: stable` | *all* channels         | *all* channels         | `beta` and `edge` only |
| `grade: devel`  | `beta` and `edge` only | `beta` and `edge` only | `beta` and `edge` only |

It's worth noting that the user of your snaps will have to use `--devmode` to install a snap using `confinement: devmode`. This means that they have to willingly accept that the snap is breaking out of confinement.

### Pushing the snap

Uploading a snap can be done either from the Snap Store dashboard or by using the command-line.

Once you have a working snap, you can run `snapcraft push snap-name.snap` to push it to the Snap Store.

### Example


    $ snapcraft push drone-autopilot_stableV2_amd64.snap
    Uploading drone-autopilot_stableV2_amd64.snap [====================] 100%
    Processing ...
    Ready to release!
    Revision 1 of 'drone-autopilot' created.

Note that `snapcraft push` will return an error if you try to push a snap with a name you haven't registered first.

### Revisions

Each time you upload a snap, the Snap Store will assign a revision number to it, starting at 1. This revision number will be incremented each time you upload a new version of your snap. The revision number also increments when uploading a build for a new architecture. 

## 4. Release your snap

Uploading a snap doesn't make it immediately available for installation. You have to choose the channel(s) you want to release into.

### Release channels

There are four channels available for your snap:

*   `stable` is what most users will consume and as the name suggests, should be your most polished, stable and tested versions. Snaps in this channel appear in user searches.
*   `candidate` is used to vet uploads that should require no further code changes before moving to stable.
*   `beta` is used to provide preview releases of semi-stable changes. Snaps requiring the `devmode` flag to work are allowed in this channel.
*   `edge` is for your most recent changes, probably untested and with no guarantees attached. Snaps requiring the `devmode` flag to work are allowed in this channel.

The same revision of a snap can be released into several channels at once.

### Release process

Releasing can be done from the Snap Store dashboard or directly from the command-line.

To release a snap, run `snapcraft release snap-name revision channel`.

### Example

    $ snapcraft release drone-autopilot 1 stable
    The 'stable' channel is now open.

    Channel    Version    Revision
    ---------  ---------  ----------
    stable     stableV2   1
    candidate  -          -
    beta       -          -
    edge       -          -

Your snap is now available for users to install and in case of a `stable` release, it will be listed in search results.

    $ snap find drone
    Name             Version   Developer  Notes  Summary
    drone-autopilot  stableV2  you        -      An autopilot mode for your drones
