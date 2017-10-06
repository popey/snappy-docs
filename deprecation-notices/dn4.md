---
layout: base
title: DN4
---

**The `history` command has been renamed to `list-revisions`**

_introduced in snapcraft 2.28_

Snapcraft has a `history` command, the output of which looks something like this:

```
$ snapcraft history my-snap
Rev.    Uploaded              Arch    Version    Channels
2       2016-09-27T19:23:40Z  i386    2.0.2      -
1       2016-09-27T18:38:43Z  amd64   2.0.1      stable*, edge
```

This is really just a list of revisions rather than a full history of
the snap (e.g. you don't see when a revision was released in a
specific channel, etc.). In order to better correspond with its purpose,
the `history` command has now been deprecated and replaced with
`list-revisions` (with a shorter `revisions` alias).