---
layout: base
title: Hooks
---

There are a number of situations where snapd needs to notify a snap that something has happened. For example, when a snap is installed, or before it is removed, you may want to run a set of setup or clean up scripts. These types of situations are handled by hooks.

Hooks are executables shipped by a snap and triggered at various points during its lifecycle. They can be used to manually change the configuration of an application and can automatically run during the install and the removal of a snap.

## Overview

There are three hooks types:

* [`configure`](#configure): run after snap installation, refresh and on demand to `snap get` or `snap set` a configuration value inside a snap.
* [`install`](#install): run when a snap is installed for the first time on the system, before any services contained by the snap have been started.
* [`remove`](#remove): run before the last revision of a snap is removed from the system.

To create a hook:

* Create a `hooks/` sub-directory in the `snap/` directory of your project
* Name your hook file after the hook type you want to use (eg. `snap/hooks/install`)
* Ensure the hook is executable (`chmod +x snap/hooks/install`)

### Confinement

By default, hooks run under the same [confinement](/reference/confinement) as the snap.

You can grant them a different confinement by declaring your hooks and their plugs in the `snapcraft.yaml` file of the project, under a top-level `hooks` key, quite similar to the `apps` key.

#### Syntax

``` yaml
hooks:
  <hook name>:
    plugs: <array of plugs>
```

#### Example

``` yaml
[...]
apps:
  my-app:
    plugs: [home, network, x11]

hooks:
  install:
    plugs: [home, network, upower-observe]
  configure:
    plugs: [home, network, upower-observe]
[...]
```

In this example, we have an application requesting power status information during install or subsequent configuration. We are therefore granting access to the `upower-observe` interface to the install and configure hooks, but denying them access to the `x11` one since they don't need to display anything on screen. Note that interfaces that are not auto-connected will not be available through hooks until they are connected.

## Hooks types

### “configure”

The configure hook is called upon initial snap install (after any services contained by the snap have started), after a refresh, and whenever the user requests a configuration change via the `snap set` command. If the hook exits non-zero, the configuration will not be applied.

#### snap set / snap get

The `snap set` and `snap get` commands to write or read a configuration value inside a snap through a "configure" hook are operated the following way:

``` bash
snap set <snap name> <key>=<value>
snap get <snap name> <key>
```

Note that the configure hook is called without parameters. From within a hook, configuration values passed by the user can be retrieved with the `snapctl get <key>` command, which implies the following order of operation for a `snap set` command:

1. The user runs `snap set some-app username=foo` to change the "username" value in the some-app snap.
2. snapd runs the configure hook shipped with some-app.
3. The configure hook iterates over possible configuration keys (which `username` is part of) by calling `snapctl get <key>` and does something with them (eg. update a configuration file of the application).

#### Example

This is an example configure hook you can re-use to _get_ and _set_ values to a config file written in the `key=value` format.

Before running the `snapcraft` command to build your snap, this `configure` file should be placed at this location:

`<snap project>/snap/hooks/configure`.

``` bash
#!/bin/sh
set -e

# CONFIG FILE PATH
CONFIG_FILE=$SNAP_DATA/config

# CONFIG OPTIONS: username, port, something-else
OPTIONS="username port something-else"

## add or replace an option inside the config file. Create the file if doesn't exist
refresh_opt_in_config() {
    opt=$1
    value="$2"
    replace_line="$opt=$value"
    if $(grep -q "$opt=" $CONFIG_FILE); then
        sed "s/^$opt=.*/$replace_line/" $CONFIG_FILE 2>/dev/null >${CONFIG_FILE}.new
        mv -f ${CONFIG_FILE}.new $CONFIG_FILE 2>/dev/null
    else
        echo $replace_line >> $CONFIG_FILE
    fi
}

# Iterate through the config options array
for opt in $OPTIONS
    do
    # Use snapctl to get the value registered by the snap set command
    refresh_opt_in_config $opt $(snapctl get $opt)
done
```

With this example, running `snap set some-app username=foo` results in the creation or the update of a config file inside the snap, containing "username=foo", which can be consumed by the application.


### “install”

The install hook is called upon initial install, before any services contained by the snap are started. If the hook exits non-zero, the installation of the snap will fail.

#### Example

Before running the `snapcraft` command to build your snap, this `install` file should be placed at this location:

`<snap project>/snap/hooks/install`.

``` bash
#!/bin/sh
set -e

# Run any commands needed to setup the snap.
# This could be downloading a list of dynamic configuration options or creating a database.
```

<br>

### “remove”

The remove hook is called when the last revision of a snap gets removed.

#### Example

Before running the `snapcraft` command to build your snap, this `remove` file should be placed at this location:

`<snap project>/snap/hooks/remove`.

``` bash
#!/bin/sh
set -e

# Run any commands needed to clean up the snap installation.
# This could be sending metrics, or gracefully stopping a service.
```