---
layout: base
title: Release your snap
---

Uploading a snap doesn't make it immediately available for installation. You have to choose the channel(s) you want to release into.

## Release channels

You’ll need to make sure when you [upload your snap](upload) that your snapcraft.yaml has the correct `confinement` and `grade` to release to a channel.

There are generally four channels available for your snap:

* `stable` is what most users will consume and as the name suggests, should be your most polished, stable and tested versions. Snaps in this channel appear in user searches.
* `candidate` is used to vet uploads that should require no further code changes before moving to stable.
* `beta` is used to provide preview releases of semi-stable changes. Snaps requiring the `devmode` flag to work are allowed in this channel.
* `edge` is for your most recent changes, probably untested and with no guarantees attached. Snaps requiring the `devmode` flag to work are allowed in this channel.

The same revision of a snap can be released into several channels at once.

More fine-grained channels are also available if you are supporting several versions of the same software, see [the channels reference](/reference/channels) for details.

## Release process

Releasing can be done from the Snap Store dashboard or directly from the command-line.

To release a snap, run `snapcraft release <snap name> revision channel`.

### Example

```
snapcraft release drone-autopilot 1 stable
```

Once the release process is complete, snapcraft will return a map of your channels:

```
The 'stable' channel is now open.

Channel    Version    Revision
---------  ---------  ----------
stable     stableV2   1
candidate  -          -
beta       -          -
edge       -          -
```

Your snap is now available for users to install and in case of a `stable` release, it will be listed in search results.

You can see for yourself with the `snap find <query>` command.

```
snap find drone
```

Which will return a table of results:

```
Name             Version   Developer  Notes  Summary
drone-autopilot  stableV2  you        -      An autopilot mode for your drones
...
```

## What comes next?

You can customise how your snaps are presented or review your uploads using the [dashboard](https://dashboard.snapcraft.io)
