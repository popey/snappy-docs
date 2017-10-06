---
layout: base
title: The nodejs plugin
---

The nodejs plugin is useful for node/npm based parts.

The plugin uses node to install dependencies from `package.json`. It
also sets up binaries defined in `package.json` into the `PATH`.

### Plugin-specific keywords

    - node-packages:
      (list)
      A list of dependencies to fetch using npm.
    - node-engine:
      (string)
      The version of nodejs you want the snap to run on.
    - npm-run:
      (list)
      A list of targets to `npm run`.
      These targets will be run in order, after `npm install`

[Examples on GitHub](https://github.com/search?o=desc&q=filename%3Asnapcraft.yaml+%22plugin%3A+nodejs%22+&s=indexed&type=Code&utf8=%E2%9C%93)