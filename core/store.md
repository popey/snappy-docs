---
layout: base
title: Snap stores
---

There are multiple ways to distribute snaps as the format is not tied to a specific distribution system. However, the best way to distribute snaps is to release them in the Snap Store. Here they can be installed by using the command line for systems that are running [snapd](/core/install). Once the snap is in the stable channel users can find your snap in the [snap directory](https://snapcraft.io/discover) or by using Gnome Software.

## The Snap Store

The main way of distributing snaps is through the [Snap Store dashboard](https://dashboard.snapcraft.io), where you can customize how snaps are presented, review each new pushed snap, and control the release process over several release channels. Here is the model it follows:

### 1. Developer namespace

You'll choose a unique developer namespace as part of the store account creation process. This namespace will represent you as a publisher and you won't be able to change it afterwards.

### 2. Naming

You can release a snap under any name you have rights to. Names can be registered by using the [`snapcraft register`](/build-snaps/register) command, or by visiting the [Register new snap](https://dashboard.snapcraft.io/snaps/register/) dashboard page. You can also grant other developers permission to release versions of a snap you own, for example as part of an open source project.

### 3. Pushing

Pushing to the Snap Store is done directly from the command-line with the [`snapcraft push`](/build-snaps/upload) command.

It's worth noting that when you push a snap, the Snap Store assigns it a revision number of 1\. The Snap Store then automatically increments this revision number each time you push a new version.

### 4. Releasing

After pushing it, your snap is reviewed by way of automated checks. If your app uses sensitive [interfaces](/core/interfaces), it may be manually reviewed and you will receive an email notifying you of the review state.

Once your snap has been reviewed and approved, you can release it using the [`snapcraft release`](/build-snaps/release) command, instantly making the snap available to users.

#### Release channels

Snaps can be released into multiple [channels](/reference/channels) (`stable`, `candidate`, `beta`, and `edge`). This enables you to engage with users who are willing to test changes, and helps users decide how close to the leading edge of development they want to be.

By default, snaps are installed from the `stable` channel. Versions of snaps from other channels need to be explicitly selected:

    $ snap install hello --channel=beta
    $ hello
    Hello, snap padawan!

A snap can be refreshed from a different channel from the one it was originally installed from:

    $ snap refresh hello --channel=edge
    Name    Version   Rev   Developer   Notes
    hello   2.10.1    29    canonical   -
    hello  (edge) installed

## Brand stores

Brands can also take advantage of white label [brand stores](https://docs.ubuntu.com/core/en/build-store/index), which are branded extensions to the Snap store. Note that this is a commercial offering aimed at brands.
