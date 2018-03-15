---
layout: base
title: Multiple snap versions and garbage collection
---

When a snap package is updated, the old version is kept. This lets `snapd` roll back to a previous, known-good version if issues are detected in the new version.

These old versions take up disk space, so garbage collection is performed automatically.

## The update process

When a snap is updated, the latest version becomes the active snap file. The content from the previous snap version’s writeable areas (`SNAP_USER_DATA` and `SNAP_DATA`) are copied to a new location, for use by the updated snap.

![Garbage collection removes older snap files ](../media/garbage_collection.png)

Garbage collection then removes and purges any snap files, and their writable areas, for snap versions prior to the one that has just been updated — meaning that, at most, two versions of a snap will be present on the system. This saves disk space without compromising the ability to revert the snap to a previous known-good state.

Explicitly removing a snap from your system will also remove the code and purge the data for all prior versions.

## Example

To illustrate the process, take the example of installing and updating `hello-world` through a few versions. If you have version `1.0.1` installed, and do a `snap refresh` that downloads version `1.0.2`:

    $ sudo snap refresh
    64.00 KB / 64.00 KB [======================] 100.00 % 4.62 KB/s    
    Name                 Version               Rev  Developer  Notes
    hello-world          1.0.2                 29   canonical  -

    $ snap list | grep hello
    hello-world          1.0.2                 29   canonical  -
    $ snap list --all | grep hello
    hello-world          1.0.1                 10   canonical  disabled
    hello-world          1.0.2                 29   canonical  -

So, `1.0.2`, was downloaded and made active, leaving `1.0.1` installed. After a further update:

    $ sudo snap refresh
    64.00 KB / 64.00 KB [======================] 100.00 % 4.62 KB/s
    Name                 Version               Rev  Developer  Notes
    hello-world          1.0.3                 32   canonical  -

    $ snap list --all | grep hello
    hello-world          1.0.2                 29   canonical  disabled
    hello-world          1.0.3                 32   canonical  -

`1.0.2` and `1.0.3` are now installed, and `1.0.1` is gone.
