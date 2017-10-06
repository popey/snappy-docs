---
layout: base
title: Snapcraft plugins reference
---

This section references all the available snapcraft plugins to build your parts.

Usage snippets can be found in the Playpen GitHub repository. As an example, here is a search that lists [all `cmake` plugin snippets](https://github.com/ubuntu/snappy-playpen/search?utf8=%E2%9C%93&q=%22plugin%3A+cmake%22&type=Code).

Note that you can run `$ snapcraft help <plugin>` to access the following documentation from the command-line.

## General topics

*   [Common keywords](/reference/plugins/common)
*   [Common 'source' options](/reference/plugins/source)

## Build systems

[kbuild](/reference/plugins/kbuild), [make](/reference/plugins/make), [qmake](/reference/plugins/qmake), [autotools](/reference/plugins/autotools), [cmake](/reference/plugins/cmake), [scons](/reference/plugins/scons), [meson](/reference/plugins/meson), [waf](/reference/plugins/waf)

## Languages

*   Java: [ant](/reference/plugins/ant), [gradle](/reference/plugins/gradle), [maven](/reference/plugins/maven)
*   Python: [python](/reference/plugins/python), [python2](/reference/plugins/python2), [python3](/reference/plugins/python3)
*   Node: [gulp](/reference/plugins/gulp), [nodejs](/reference/plugins/nodejs)
*   Go: [go](/reference/plugins/go), [godeps](/reference/plugins/godeps)
*   Rust: [rust](/reference/plugins/rust)

## Projects

*   ROS: [catkin](/reference/plugins/catkin)
*   Linux: [kernel](/reference/plugins/kernel)

## Utilities

[nil](/reference/plugins/nil), [dump](/reference/plugins/dump)