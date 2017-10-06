---
layout: base
title: DN2
---

**Aliases are now handled by the Snap Store, and shouldn't be placed in the snap.**

_introduced in snapcraft 2.35_

Originally aliases were something declared by the snap developer by actually
coding it into the snap. A few problems manifested with this approach:

1. Users could not add aliases that were not explicitly added by the snap
   author.
2. Aliases could be selected piecemeal by the Snap Store and by users, which was
   flexible but too heavy for the common case of just getting software to work.

To solve these issues, aliases are no longer carried within the snap itself
with the `aliases` property, but handled completely by the Snap Store. Users are
free to setup aliases under their control (via `snap alias`), and automatic
aliases are also handled as a block, being enabled or disabled together.

To get aliases in the Snap Store, the developer needs to request them and get
approval from the developer community. This process happens in the Snapcraft
forum; see the [request guidelines][1].

[1]: https://forum.snapcraft.io/t/process-for-reviewing-aliases-auto-connections-and-track-requests