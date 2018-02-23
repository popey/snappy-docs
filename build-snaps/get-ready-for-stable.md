---
layout: base
title: Get ready for stable
---

Snaps can be [released](/build-snaps/release.md) in the store to one or more [channels](/reference/channels). By default only the `stable` channel is publicly searchable. 

One byproduct of this is developers can 'hide' unfinished applications, or snaps which aren't production quality from users. However, once an application hits the stable channel, users will have expectations for functionality and presentation. 

This page details some tips on getting your application ready for the stable snap store channel. Consider it a checklist of best practices for landing in the stable channel.

## Confinement

Snaps in the stable channel must either conform to `strict` confinement, or with approval, `classic` confinement. In general applications should be using `strict` confinement. 

In the event an application cannot be strictly confined, we recommend starting a thread on the [forum](https://forum.snapcraft.io/) to discuss a way forward. If `classic` is required, check the [process](https://forum.snapcraft.io/t/process-for-reviewing-classic-confinement-snaps/1460) for reviewing classic confined snaps to check if an exception can be made, and then start a request thread on the [forum](https://forum.snapcraft.io/).

## Interfaces

When strictly confining your application, ensure you've specified all required [interfaces](/reference/interfaces). You may have specified interfaces which are *not* auto-connected. If you believe they *should* be automatically connected on end-user computers, start a thread on the [forum](https://forum.snapcraft/io) requesting them. 

Those will each be discussed and reviewed in the open. In the event they're approved, a store assertion will be setup so users installing your snap do not need to manually `snap connect` those interfaces.

## Aliases

If the snap contains only one binary exposed to the host, and that binary has the same name as the snap, there is nothing to do here. For example the `pulsemixer` contains only the `pulsemixer` binary.

However if you want the snap to expose multiple binaries to the host system, they will by default all be prefixed with the snap name. So a snap called `atom` which contains two binaries - `atom` and `apm` will by default expose `atom` and `atom.apm` to the host where the snap is installed.

In this case an alias for `apm` was [requested](https://forum.snapcraft.io/t/auto-alias-apm-in-atom-snap/2393) for the `atom` snap so users can invoke the `apm` command via `apm` directly.

## Presentation

### Store Icon

The snap store supports a single icon for the application, which should be a 256x256 png or jpeg. The icon can be uploaded from the presentation page in the [dashboard](https://dashboard.snapcraft.io/). The icon uploaded here will show in the [snapcraft.io](https://snapcraft.io/) [discover](https://snapcraft.io/discover) pages and other graphical store fronts.

### Desktop Icon

If the application is a graphical desktop app, then the snap should ship with an icon and `.desktop` file. When building the snap, alongside the `snap/snapcraft.yaml` file, add a `snap/gui/snapname.png` and `snap/gui/snapname.desktop`, where "snapname" matches the `name:` entry in the `snapcraft.yaml`.

The `.desktop` file should refrence the executable simply as

```Exec=snapname```

Where "snapname" matches the executable in the `apps:` section of the `snapcraft.yaml` or an approved alias.

The path to the icon should also be set in the `.desktop` file as:

```Icon=${SNAP}/meta/gui/snapname.png```

During installation, the icon and desktop file will be exposed to the userss' desktop environment.

### Screenshots

The snap store supports uploading multiple screenshots. End users wil see those screenshots in desktop applications such as GNOME Software. We recommend uploading at least one, and up to four screenshots via the [dashboard](dashboard.snapcraft.io).

### Banner

An optional banner may be shown in GNOME Software when an application has been selected by the store admins to be featured. The banner should be uploaded in the same place as other screenshots, but must be called `banner.png` or `banner.jpg` and must be 1218x240 pixels in size.

Uploading a banner does not guarantee the application will be featured, but if it is, the banner will improve the appearance.

### Description

The application description is shown in the graphical snap store, via `snap info` and on the [snapcraft.io](https://snapcraft.io/) [discover](https://snapcraft.io/discover) pages. Itaccurately describe the application. 

The contents originates in the `snapcraft.yaml` `description:` field, and can be updated via the `snapcraft push-metadata` command. In addition, the description can be maintained in the store [dashboard](https://dashboard.snapcraft.io/).

### Support link

The support link in the store enables users to provide feedback about your application. It should point to a support site, forum or bug tracker. This field is maintained in the store [dashboard](https://dashboard.snapcraft.io/) General -> Edit -> Contact URL.


