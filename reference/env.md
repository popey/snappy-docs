---
layout: base
title: Environment variables
---

This page documents various environment variables available to snap applications.

## List environment variables

To have an overview of the environment of a specific snap, you can enter its shell and run the `env` command:

    $ snap run --shell <snap>.<command>
    $ env
    SNAP_USER_COMMON=/home/<user>/snap/<snap>/common
    SNAP_LIBRARY_PATH=/var/lib/snapd/lib/gl:
    SNAP_COMMON=/var/snap/<snap>/common
    [...]

## Snap specific environment variables

### `SNAP_NAME`

The name of the snap as specified in the `snapcraft.yaml` file.

Typical value: `hello-world`

### `SNAP_VERSION`

The version string as specified in the `snapcraft.yaml`

Typical value `6.3`

### `SNAP_REVISION`

Revision of the snap, as allocated by the Snap Store on upload or as allocated by snapd for locally installed snaps.

The Snap Store assigns monotonic revisions to each upload of a given snap. Snapd uses Snap Store revisions if accompanying assertions are available or uses a locally generated number. Locally generated numbers are prefixed with `x` to distinguish them from Snap Store uploads.

Typical value: `27` or `x1`

### `SNAP_ARCH`

CPU architecture of the running system.

Typical value `amd64`

Other values are: `i386`, `armhf`, `arm64`.

### `SNAP`

Directory where the snap is mounted. This is where all the files in your snap are visible in the filesystem.
All of the data in the snap is read-only and cannot be changed.

Typical value: `/snap/hello-world/27`

### `SNAP_USER_DATA`

Directory for user data.

This directory is backed up and restored across `snap refresh` and `snap revert` operations.

Typical value: `/home/zyga/snap/hello-world/27`

The final number there is `$SNAP_REVISION`.

### `SNAP_USER_COMMON`

Directory for user data that is common across revisions of a snap.

Unlike `SNAP_DATA`, data present in this directory is not backed up or restored across `snap refresh` and `snap revert` operations. The directory is suitable for large data that the application can access even if it was made or modified by a future version of a snap.

Typical value `/home/zyga/snap/hello-world/common`

### `SNAP_DATA`

Directory for system data of a snap.

This directory is owned and writable by `root` and is meant to be used by background applications (daemons, services). Unlike `SNAP_COMMON` this directory is backed up and restored across `snap refresh` and `snap revert` operations.

Typical value `/var/snap/hello-world/27`

### `SNAP_COMMON`

Directory for system data that is common across revisions of a snap.

This directory is owned and writable by `root` and is meant to be used by background applications (daemons, services). Unlike `SNAP_DATA` this directory does is not backed up and restored across snap refresh and revert operations.

Typical value: `/var/snap/hello-world/common`

### `SNAP_LIBRARY_PATH`

Directory with additional system libraries. This variable is used internally by snapcraft.

The value is always `/var/lib/snapd/lib/gl:` Please note the colon at the end of that value, the variable is a colon-separated list.

The referenced directory is typically empty unless Nvidia proprietary drivers are in use.

## Generic variables

### `HOME`

This environment variable is re-written by snapd so that each snap appears to have a dedicated home directory that is a subdirectory of the real home directory.

Typical value: `/home/zyga/snap/hello-world/27`

### `PATH`

This environment variable is re-written by snapd so that it is consistent with the view of the filesystem presented to snap applications.

The value is always: `/usr/sbin:/usr/bin:/sbin:/bin:/usr/games`