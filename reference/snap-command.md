---
layout: base
title: 'snap: command reference'
---

* `help`: Shows the help content
* `version`: Prints the version

## Store commands

* [`find`:](#find)       Finds packages to install
* [`login:`](#login)       Authenticates on snapd and the Snap Store
* [`logout:`](#logout)      Log out of the Snap Store
* [`buy:`](#buy)         Buys a snap

## Manage snaps on a system

* [`install:`](#install)     Installs a snap to the system
* [`remove:`](#remove)      Removes a snap from the system
* [`list:`](#list)        List installed snaps
* [`info:`](#info)        show detailed information about a snap
* [`refresh:`](#refresh)     Refreshes a snap in the system
* [`revert:`](#revert)      Reverts the given snap to the previous state
* [`disable:`](#disable)     Disables a snap in the system
* [`enable:`](#enable)      Enables a snap in the system
* [`alias:`](#alias)      Enables the given aliases
* [`unalias:`](#unalias)       Disables the given aliases

## Manage interfaces connections

* [`interfaces:`](#interfaces)  Lists interfaces in the system
* [`connect:`](#connect)     Connects a plug to a slot
* [`disconnect:`](#disconnect)  Disconnects a plug from a slot

## See system changes

* [`changes:`](#changes)     List system changes
* [`watch:`](#watch)       Watch a change in progress
* [`abort:`](#abort)       Abort a pending change
* [`change:`](#change)      List a change's tasks

## Developer commands

* [`try:`](#try)         Tests a snap in the system
* [`run:`](#run)         Run the given snap command
* [`download:`](#download)    Downloads the given snap
* [`get:`](#get)         Prints configuration options
* [`set:`](#set)         Changes configuration options
* [`known:`](#known)       Shows known assertions of the provided type
* [`ack:`](#ack)         Adds an assertion to the system

### find

Usage: `snap find <query>`

The find command queries the Snap Store for available packages.

#### Command options
 * `--private`:  Search private snaps
 * `--section=`: Restrict the search to a given section

### login

Usage: `snap login <email>`

The login command authenticates on snapd and the Snap Store and saves credentials
into the `~/.snap/auth.json` file. Further communication with snapd will then be made using those credentials.

Login only works for local users in the `sudo`, `admin` or `wheel` groups.

An account can be setup at [https://login.ubuntu.com](https://login.ubuntu.com)

### logout

Usage: `snap logout`

This command logs the current user out of the Snap Store

### buy

Usage: `snap buy <snap>`

The buy command buys a snap from the Snap Store.

#### Command options

 * `--currency=`: [ISO 4217 code](https://en.wikipedia.org/wiki/ISO_4217) for currency

### install

Usage: `snap install <snap>`

The install command installs the named snap in the system.

#### Command options

 * `--channel=`:         Use this channel instead of stable
 * `--edge`:             Install from the edge channel
 * `--beta`:             Install from the beta channel
 * `--candidate`:        Install from the candidate channel
 * `--stable`:           Install from the stable channel
 * `--devmode`:          Put snap in development mode and disable security confinement
 * `--jailmode`:         Put snap in enforced confinement mode
 * `--classic`:          Put snap in classic mode and disable security confinement
 * `--revision=`:        Install the given revision of a snap, to which you must have developer access
 * `--dangerous`:        Install the given snap file even if there are no pre-acknowledged signatures for it, meaning it was not verified and could be dangerous (`--devmode` implies this)

### remove

Usage: `snap remove <snap>`

The remove command removes the named snap from the system.

By default all the snap revisions are removed, including their data and the common
data directory. When a `--revision` option is passed only the specified revision is
removed.

#### Command options

 * `--revision=`: Remove only the given revision

### list

Usage: `snap list <snap>`

The list command displays a summary of snaps installed in the current system.

#### Command options

 *           `--all`: Show all revisions

### info

Usage:
   `snap info <snap>`

The info command shows detailed information about a snap, be it by name or by path.

#### Command options
 * `--verbose`:  Include a verbose list of a snap's notes (otherwise, summarise notes)

### refresh

Usage:
   `snap refresh <snap>`

The refresh command refreshes (updates) the named snap.

#### Command options
 *         `--channel=`:           Use this channel instead of stable
 *         `--edge`:               Install from the edge channel
 *         `--beta`:               Install from the beta channel
 *         `--candidate`:          Install from the candidate channel
 *         `--stable`:             Install from the stable channel
 *         `--devmode`:            Put snap in development mode and disable security confinement
 *         `--jailmode`:           Put snap in enforced confinement mode
 *         `--classic`:            Put snap in classic mode and disable security confinement
 *         `--revision=`:          Refresh to the given revision
 *         `--list`:               Show available snaps for refresh
 *         `--ignore-validation`:  Ignore validation by other snaps blocking the refresh

### revert

Usage:
   `snap revert <snap>`

The revert command reverts the given snap to its state before
the latest refresh. This will reactivate the previous snap revision,
and will use the original data that was associated with that revision,
discarding any data changes that were done by the latest revision. As
an exception, data which the snap explicitly chooses to share across
revisions is not touched by the revert process.

[revert command options]
* `--devmode`:          Put snap in development mode and disable security confinement
* `--jailmode`:         Put snap in enforced confinement mode
* `--classic`:          Put snap in classic mode and disable security confinement
* `--revision=`:        Revert to the given revision

### disable

Usage:
   `snap disable <snap>`

The disable command disables a snap. The binaries and services of the
snap will no longer be available. But all the data is still available
and the snap can easily be enabled again.

### enable

Usage:
   `snap enable <snap>`

The enable command enables a snap that was previously disabled.


### alias

Usage:
   `snap alias <snap>`

The alias command enables the given application aliases defined by the snap.

Once enabled the respective application commands can be invoked just using the aliases.

#### Command options
 *         `--reset=`:           Reset the aliases to their default state, enabled for automatic aliases, disabled otherwise

### unalias

Usage:
    `snap unalias <snap> <alias>`

The unalias command disables explicitly the given application aliases defined by the snap.

### interfaces

Usage:
   `snap interfaces <snap>:<slot or plug>`

The interfaces command lists interfaces available in the system.

By default all slots and plugs, used and offered by all snaps, are displayed.

    $ snap interfaces <snap>:<slot or plug>

Lists only the specified slot or plug.

    $ snap interfaces <snap>

Lists the slots offered and plugs used by the specified snap.

    $ snap interfaces -i=<interface> <snap>

Filters the complete output so only plugs and/or slots matching the provided details are listed.

#### Command options
 *       `-i=`:                          Constrain listing to specific interfaces

### connect

Usage:
   `snap connect <snap>:<plug> <snap>:<slot>`

The connect command connects a plug to a slot.
It may be called in the following ways:

    $ snap connect <snap>:<plug> <snap>:<slot>

Connects the provided plug to the given slot.

    $ snap connect <snap>:<plug> <snap>

Connects the specific plug to the only slot in the provided snap that matches
the connected interface. If more than one potential slot exists, the command
fails.

    $ snap connect <snap>:<plug>

Connects the provided plug to the slot in the core snap with a name matching
the plug name.

### disconnect

Usage:
   `snap disconnect <snap>:<plug> <snap>:<slot>`

The disconnect command disconnects a plug from a slot.
It may be called in the following ways:

    $ snap disconnect <snap>:<plug> <snap>:<slot>

Disconnects the specific plug from the specific slot.

    $ snap disconnect <snap>:<slot or plug>

Disconnects everything from the provided plug or slot.
The snap name may be omitted for the core snap.

### changes

Usage:
   `snap changes <snap>`

The changes command displays a summary of the recent system changes performed.

### watch

Usage:
   `snap watch <change-id>`

The watch command waits for the given change-id to finish and shows progress
(if available).

### abort

Usage:
   `snap abort <change-id>`

The abort command attempts to abort a change that still has pending tasks.

### change

Usage:
   `snap change <id>`

The change command displays a summary of tasks associated to an individual change.

### try

Usage:
   `snap try <snap-dir>`

The `try` command installs an unpacked snap into the system for testing purposes.
The unpacked snap content continues to be used even after installation, so
non-metadata changes there go live instantly. Metadata changes such as those
performed in snap.yaml will require reinstallation to go live.

If `snap-dir` argument is omitted, the try command will attempt to infer it if
either snapcraft.yaml file and prime directory or meta/snap.yaml file can be
found relative to current working directory.

#### Command options
* `--devmode`:          Put snap in development mode and disable security confinement
* `--jailmode`:         Put snap in enforced confinement mode
* `--classic`:          Put snap in classic mode and disable security confinement

### run

Usage:
   `snap run <snap>.<command>`

Run the given snap command with the right confinement and environment

#### Command options
 *        ` --shell`:    Run a shell instead of the command (useful for debugging)

### download

Usage:
   `snap download <snap>`

The download command downloads the given snap and its supporting assertions
to the current directory under .snap and .assert file extensions, respectively.



#### Command options
 *         `--channel=`:   Use this channel instead of stable
 *         `--edge`:       Install from the edge channel
 *         `--beta`:       Install from the beta channel
 *         `--candidate`:  Install from the candidate channel
 *         `--stable`:     Install from the stable channel
 *         `--revision=`:  Download the given revision of a snap, to which you must have developer access

### get

Usage:
   `snap get <snap> <key>`

The get command prints configuration options for the provided snap.

    $ snap get snap-name username
    frank

If multiple option names are provided, a document is returned:

    $ snap get snap-name username password
    {
    "username": "frank",
    "password": "..."
    }

Nested values may be retrieved via a dotted path:

    $ snap get snap-name author.name
    frank

#### Command options
 *       `-t`:            Strict typing with nulls and quoted strings
 *       `-d`:            Always return document, even with single key


### set

Usage:
   `snap set <snap> <key=value>`

The set command changes the provided configuration options as requested.

    $ snap set snap-name username=frank password=$PASSWORD

All configuration changes are persisted at once, and only after the
snap's configuration hook returns successfully.

Nested values may be modified via a dotted path:

    $ snap set author.name=frank

### known

Usage:
   `snap known <assertion type> <header=value>`

The known command shows known assertions of the provided type.
If `header=value` pairs are provided after the assertion type, the assertions
shown must also have the specified headers matching the provided values.


#### Command options
 *         `--remote`

### ack

Usage:
   `snap ack <assertion file>`

The `ack` command tries to add an assertion to the system assertion database.

The assertion may also be a newer revision of a preexisting assertion that it
will replace.

To succeed the assertion must be valid, its signature verified with a known
public key and the assertion consistent with and its prerequisite in the
database.