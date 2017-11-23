---
layout: base
title: Upload your snap
---

Before you upload your snap, have a quick look at your `snapcraft.yaml` file again. Two settings (`grade` and `confinement`) will define which [channels](/reference/channels) you can release your snap to.

|                 | `confinement: strict`  | `confinement: classic` | `confinement: devmode` |
| --------------- | ---------------------- | ---------------------- | ---------------------- |
| `grade: stable` | *all* channels         | *all* channels         | `beta` and `edge` only |
| `grade: devel`  | `beta` and `edge` only | `beta` and `edge` only | `beta` and `edge` only |

It's worth noting that the user of your snaps will have to use `--devmode` to install a snap using `confinement: devmode`. The same principle applies to `classic` confinement. This means that they have to willingly accept that the snap is breaking out of confinement.

## Pushing the snap

1. `cd` to the directory containing your snap file
2. Run the `snapcraft push` command, appending your snap's version and architecture

### Example

```
snapcraft push drone-autopilot_stableV2_amd64.snap
```
Your snap package will then be pushed and processed by the store:

```
Uploading drone-autopilot_stableV2_amd64.snap [====================] 100%
Processing ...
Ready to release!
Revision 1 of 'drone-autopilot' created.
```

Note that `snapcraft push` will return an error if you try to push a snap with a name you haven't registered first.

## Revisions

Each time you upload a snap, the Snap Store will assign a revision number to it, starting at 1. This revision number will be incremented each time you upload a new version of your snap. The revision number also increments when uploading a build for a new architecture.

## Need more information?

Use these commands for more information about uploading your snap

- `snapcraft push --help`
- `snapcraft status <snap name>`
- `snapcraft list-revisions <snap name>`

## What's next?

[Releasing your snap](release)
