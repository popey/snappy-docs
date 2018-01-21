---
layout: base
title: Snap stores
---

Given the simplicity and flexibility of the snap format, there are many ways to distribute snap applications. Using the Snap Store exposes your application to tens of millions of potential users and gives you detailed install statistics, so using it is recommended. Once the snap is in the stable channel users can find your snap in the [snap directory](https://snapcraft.io/discover) or by using Gnome Software.

## The Snap Store

The main way of distributing snaps is through the [Snap Store dashboard](https://dashboard.snapcraft.io), where you can customize how snaps are presented, review each new pushed snap, and control the release process over several release channels. Here is the model it follows:

### 1. Username

You'll choose a username as part of creating your Snap Store account. This unique name will represent you as a publisher, so pick a name that best reflects your brand. You will not be able to change it once set.

### 2. Naming

You can release a snap under any name you have rights to. Names can be registered by using the [`snapcraft register`](/build-snaps/register) command, or by visiting the [Register new snap](https://dashboard.snapcraft.io/register-snap/) dashboard page. You can also grant other developers permission to release versions of a snap you own on the dashboard "Collaboration" page.

### 3. Pushing

Pushing to the Snap Store is done directly from the command-line with the [`snapcraft push`](/build-snaps/upload) command.

It's worth noting that when you push a snap, the Snap Store assigns it a revision number of 1\. The Snap Store then automatically increments this revision number each time you push a new version.

### 4. Releasing

After pushing your snap, it is reviewed by way automated checks. Most automated reviews pass with no further action needed.

Use of some [interfaces](/core/interfaces) will trigger a manual review. You will receive emails explaining any further action needed and the state of the review process.

Once your snap has been reviewed and approved, you can release it using the [`snapcraft release`](/build-snaps/release) command, instantly making the snap available to users.

#### Release channels

Snaps can be released into multiple [channels](/reference/channels) (`stable`, `candidate`, `beta`, and `edge`). This enables you to engage with users who are willing to test changes. It lets users decide how close to the leading edge of development they want to be.

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
