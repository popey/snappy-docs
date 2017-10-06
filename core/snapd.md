---
layout: base
title: The snapd system
---

In traditional Linux distributions, software is made available in packages:

- that rely on the availability of services in the OS or other software.
- whose data isn't confined, so can be accessed by other software.
- that can be detrimentally effected by a system or software upgrade.
- that are complex to uninstall or downgrade.
- rely on a small number of approved 'packagers' to add them to the distro repositories.

Creating and distributing software can therefore be a time consuming process and the end result doesn't offer the user a high degree of security and manageability.

The snapd system aims to fix these challenges by offering:

- System components and applications as self contained (except for the most basic OS features, such as network access), read only images, called snaps.
- A confinement and security model that:

  - Offers snaps a secure storage area isolated from other snaps.
  - Enables snaps to make features available to other snaps and for other snaps to consume those features over defined interfaces.

- A store where developers can easily make their software directly available to users and from which devices can automatically pull updates on a daily basis.
- A simple transactional update system where snaps can be easily uninstalled (by deleting the snap package) or rolled back (by reverting to the previous snap image and private storage area).

On a snapd system these features are implemented by:

- **snapd**, a management environment that handles installing and updating snaps using the transactional system, as well as [garbage collection](/core/versions) of old versions of snaps.
- **snap-confine**, an execution environment for the applications and services delivered in snap packages.

![Snaps are self contained, confined applications that can make use of features in other snaps using Interfaces.](../media/snap_in_snappy_system.png "Snaps in the Snapd System")

The snapd system simplifies the development of devices and their software because, with the exception of a limited number of OS features, you're in control of all the components in your application. You simply add everything needed to the snap package. You then make the snap available using the [Snap Store](/core/store "Snap Store"), or, if you are the device creator, create your own store.

## OS snaps

The OS snap is a repacked `rootfs` that contains just 'enough' to run and manage snaps on a read-only file system. Generally there will also be basic features such as network services, libc, systemd, and others included.

When you install a snap for the first time, the OS snap (`ubuntu-core`) gets installed first, it's used as the platform for subsequently installed application snaps. This way, you can be confident that a snap always runs on the same core stack, regardless of the Linux distribution.