---
layout: base
title: The snap format
---

<!--- These entries need two spaces at the end of line to be properly formatted. -->

[Introduction](#intro)  
[Setup files](#files)  
[snap.yaml](#snap.yaml)  
[Interfaces](#interfaces)  
[Desktop files](#desktop-files)  


<a name="intro">
## Introduction

A snap is a squashfs file carrying content and a bit of metadata that tells the system how to manipulate it. When installed the squashfs file is mounted read-only under

  * `/snap/<snap name>/<revision>/`

This means fast and extremely predictable installations, with no leftovers and no way for the content to be mutated over traditional means. Either it's all installed and available as originally built, or it is not available at all.

Applications declared in the snap become commands at

  * `/snap/bin/<snap name>[.<app name>]`

The suffix is omitted if `<app name>` matches `<snap name>`. That file is not the actual application, though, but rather a command that will trigger the real application to be run under the proper isolation and confinement rules, based on the default restricted environment plus any allowances granted to it via the interface system.

<a name="files">
## Setup files

The following files control the behavior of a snap:

- `meta/snap.yaml` - Basic snap details (see below).
- `meta/hooks/` - Hooks called on specific events (see below).
- `meta/gui/icon.svg` - Icon for the snap.
- `meta/gui/*.desktop` - Desktop files for the snap (see below).

<a name="snap.yaml">
## snap.yaml

The `meta/snap.yaml` file contains the basic metadata and is a requirement for all snaps.

Most of its allowed content is optional. In fact, the simplest snap possible may have as little as this inside `snap.yaml`:
```
name: simplest
version: 1.0
```

A snap that offers an application to run is still very simple:
```
name: simple
version: 1.0
apps:
    hello:
        command: bin/hello --world
```

The following specification defines what is supported inside it:
```
# The suggested snap name, constrained to the [a-z0-9] charset and inner
# dashes. The final name when the snap is installed is defined by the
# snap-declaration assertion associated with the snap, if any.
name: <snap name>

# Version of the software packed inside the snap. Has no semantic value
# in the system (no greater/lower-than rules are ever applied to it).
version: <version>

# More details about what is contained in the snap.
summary: <line>
description: <text>

# Type of snap, defaults to "app".
type: app | core | gadget | kernel

# List of architectures the snap may run on. Defaults to [all].
architectures:
    - all | amd64 | i386 | armhf

# List of applications (commands, binaries, daemons) in the snap.
apps:

  <app name>:

      # Path to executable (relative to snap base) and arguments to use
      # when this application is run.
      command: <command line>

      # List of plug names this application is associated with.
      # When a plug is connected to one of these slots, this application
      # will be granted the permissions specified for that interface.
      # If attributes are required or the plug name does not match the
      # interface name, more details must be declared under the top-level
      # "plugs" field (see below).
      plugs:
          - <plug name>

      # List of slot names this application is associated with.
      # Same details as described above, but for slots.
      slots:
          - <slot name>

      # If daemon is set, the command is a daemon to run as specified.
      # See systemd documentation for details on those kinds.
      daemon: simple | forking | oneshot

      # Optional command to use for stopping a daemon.
      stop-command: <command line>

      # Optional time to wait for daemon to stop.
      stop-timeout: <n>ns | <n>us | <n>ms | <n>s | <n>m

      # Optional command to run after daemon stops.
      post-stop-command: <command line>

      # Condition to restart the daemon under. Defaults to on-failure.
      # See the systemd.service manual on Restart for details.
      restart-condition: \
          on-failure | on-success | on-abnormal | on-abort | always | never

      # Optional stream abstract socket name or socket path.
      # When defined as a path, it should normally be in one of the snap
      # writable directories. For an abstract socket it must start with
      # @<snap name> or @<snap name>_.
      listen-stream: <path> | @<snap name> | @<snap name>_<suffix>

      # Whether the daemon is socket activated. Defaults to false, and
      # must be defined together with listen-stream if true.
      socket: true | false

```


## Interfaces

Interfaces allow snaps to communicate or share resources according to the
protocol established by the interface and play an important part in security
policy configuration. See [Interfaces](/core/interfaces) for details.


## Desktop files

The `meta/gui/` directory may contain `*.desktop` files for the snap.
Those desktop files may contain all valid desktop entries from the
XDG Desktop Entry Specification version 1.1 with some exceptions listed
below. If there is a line with an unknown key or an unofficial key
that line is silently removed from the desktop file on install.

The `Exec=` line must necessarily look like the following to be valid:

  * `Exec=<snap name>[.<app name>] [<argument> ...]`

As in the executables contained under `/snap/bin`, the `.<app name>` suffix is
omitted if the application name and snap name are the same.

For example, assuming this content in `snap.yaml`:
```
name: http
version: 1.0
apps:
    get:
        command: bin/my-downloader
```

This desktop file is valid:
```
[Desktop Entry]
Name=My Downloader
Exec=http.get %U
```


### Unsupported desktop keys

The `DBusActivatable`, `TryExec` and `Implements` keys are currently
not supported and will be silently removed from the desktop file on
install.