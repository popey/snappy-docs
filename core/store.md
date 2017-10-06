---
layout: base
title: Snap stores
---

There are multiple ways to distribute snaps as the format is not tied to a specific distribution system.

## The Snap Store

The main way of distributing snaps to snapd systems is through the [Snap Store](https://dashboard.snapcraft.io "Snap Store"), where you can customize how your snap is presented, review each new pushed snap, and control their release process over several release channels. Here is the model it follows.

### Developer namespace

You'll choose a unique developer namespace as part of the account creation process. This namespace will represent you as a publisher in the Snap Store and you won't be able to change it afterwards.

### Naming

You can release a snap under any name you have rights to. Names can be registered by using the `snapcraft register` command, clicking **New Snap** on the developer portal, or by visiting the [Register name](https://dashboard.snapcraft.io/dev/snaps/register-name/ "register name") page. You can also grant other developers permission to release versions of a snap you own, for example as part of an open source project.

### Pushing

Pushing to the Snap Store can be done directly with the [`snapcraft push`](/build-snaps/publish "snapcraft push") command or on the [Snap Store dashboard](https://dashboard.snapcraft.io "Snap Store dashboard") itself. Once pushed, you choose the release channel(s) (`stable`, `candidate`, `beta`, `edge`) that the snap will be released into.

It's worth noting that when you push a snap, the Snap Store assigns it a revision number of 1\. The Snap Store then automatically increments this revision number each time you push a new version.

### Releasing

After you've chosen a channel, your application is sent for review. Most apps are reviewed by way of automated checks, but if your app uses sensitive [interfaces](/core/interfaces), it may be manually reviewed -- you can find more details on the review process [here](https://developer.ubuntu.com/en/publish/application-states/).

Once your snap has been reviewed and approved, you can release it when you're ready, instantly making the snap available to users.

### Release channels

On the Snap Store, snaps can be published into different channels at the same time: `stable`, `candidate` (release candidate), `beta`, and `edge`. This enables you to engage with users who are willing to test changes, and helps users decide how close to the leading edge of development they want to be.

By default, snaps are installed from the `stable` channel. Versions of snaps from other channels need to be explicitly selected:

    $ snap install hello --beta
    $ hello
    Hello, snap padawan!

And a snap can be refreshed from a different channel from the one it was originally installed from:

    $ snap refresh hello --beta
    Name    Version   Rev   Developer   Notes
    hello   2.10.1    29    canonical   -
    hello  (beta) installed

This switches the snap to using this channel for future updates.

## Brand stores

Brands can also take advantage of white label [brand stores](https://docs.ubuntu.com/core/en/build-store/index), which are branded extensions to the Snap store. Note that this is a commercial offering aimed at companies.