---
layout: base
title: Go
---

Snapcraft builds on top of the `go` tool, familiar to any Go developer, to create snaps for people to install on Linux.

## What problems do snaps solve for Go applications?

Often Linux install documentation for Go applications consists of downloading pre-built binaries (or running `go get`). When distributed this way, getting updates is an exercise left to the reader. With snapcraft it’s just one command to produce a bundle that works anywhere and can be automatically updated.
Here are some snap advantages that will benefit many Go projects:

 * Simplify installation instructions, regardless of distribution, to `snap install mygoapp`.
 * Directly control the delivery of application updates.
 * Extremely simple creation of daemons.
 * Deliver assets such as images or static web content inside the snap.

## Getting started

Let's take a look at httplab and go-ethereum by way of examples. Both are command line applications. httplab is a very simple example of a typical Go snap. go-ethereum (geth) requires a specific version of Go, and contains multiple commands.

### httplab

Snaps are defined in a single yaml file placed in the root of your project. The httplab example shows the entire `snapcraft.yaml` for an existing project. We'll break this down.

```yaml
name: httplab
version: git
summary: An interactive web server.
description: |
  HTTPLab let you inspect HTTP requests and forge responses.

grade: devel
confinement: devmode

parts:
  httplab:
    source: .
    plugin: go
    go-importpath: github.com/gchaincl/httplab

apps:
  httplab:
    command: httplab
```

#### Metadata

The `snapcraft.yaml` starts with a small amount of human-readable metadata, which usually can be lifted from the GitHub description or project README.md. This data is used in the presentation of your app in the Snap Store. The `summary:` can not exceed 79 characters. You can use a pipe with the `description:` to declare a multi-line description.

```yaml
name: httplab
version: git
summary: An interactive web server.
description: |
  HTTPLab let you inspect HTTP requests and forge responses.
```

#### Confinement

To get started we won’t confine this application. Unconfined applications, specified with `devmode`, can only be released to the hidden “edge” channel where you and other developers can install them.

```yaml
confinement: devmode
```

#### Parts

Parts define how to build your app. Parts can be anything: programs, libraries, or other assets needed to create and run your application. In this case we have one: the httplab source code. In other cases these can point to local directories, remote git repositories, or tarballs.

The Go plugin will build using the version of Go on the system running snapcraft. We’ll cover building using different versions of Go in the geth example further down.

When using local sources, snapcraft needs to construct a suitable `GOPATH`. For this it uses `go-importpath` to know where the sources should live within `GOPATH/src`.

```yaml
parts:
  httplab:
    source: .
    plugin: go
    go-importpath: github.com/gchaincl/httplab
```

#### Apps

Apps are the commands and services exposed to end users. If your command name matches the snap `name`, users will be able run the command directly. If the names differ, then apps are prefixed with the snap `name` (`httplab.command-name`, for example). This is to avoid conflicting with apps defined by other installed snaps.

If you don’t want your command prefixed you can request an alias for it on the [Snapcraft forum](https://forum.snapcraft.io/t/process-for-reviewing-aliases-auto-connections-and-track-requests/455). These are set up automatically when your snap is installed from the Snap Store.

```yaml
apps:
  httplab:
    command: httplab
```

If your application is intended to run as a service you simply add the line `daemon: simple` after the command keyword. This will automatically keep the service running on install, update, and reboot.

### Building the snap

You’ll first need to [install snap support](/core/install), and then install the snapcraft tool:
```
sudo snap install --beta --classic snapcraft
```

If you have just installed snap support, start a new shell so your `PATH` is updated to include `/snap/bin`. You can then build this example yourself:

```
git clone https://github.com/snapcraft-docs/httplab
cd httplab
snapcraft
```

The resulting snap can be installed locally. This requires the `--dangerous` flag because the snap is not signed by the Snap Store. The `--devmode` flag acknowledges that you are installing an unconfined application:

```
sudo snap install httplab_*.snap --devmode --dangerous
```

You can then try it out

```
httplab
```

Removing the snap is simple too:

```
sudo snap remove httplab
```

Jump ahead to [Share with your friends](#share-with-your-friends) or continue to read another example.

### go-ethereum (geth)

The geth example shows a `snapcraft.yaml` using a different version of Go than the one shipped in the distribution archive on the machine building the snap. Here is the entire `snapcraft.yaml` for geth. We'll break this down.

```yaml
name: geth
version: git
summary: Official Go implementation of the Ethereum protocol
description: |
    Official Go implementation of the Ethereum protocol

grade: devel
confinement: devmode

apps:
  abigen:
    command: bin/abigen
  bootnode:
    command: bin/bootnode
  evm:
    command: bin/evm
  faucet:
    command: bin/faucet
  geth:
    command: bin/geth --datadir "$SNAP_USER_COMMON/ethereum"
  puppeth:
    command: bin/puppeth
  rlpdump:
    command: bin/rlpdump
  swarm:
    command: bin/swarm
  wnode:
    command: bin/wnode

parts:
  go:
    source-tag: go1.7.5
  geth:
    after: [go]
    source: .
    plugin: go
    go-importpath: github.com/ethereum/go-ethereum
```

#### Parts

In the geth example we have two parts, one for geth itself, and a second which pulls a newer version of Go.

The geth part specifies that it must be run 'after' the 'go' part. This ensures that when geth is built, it is done so with the newer release of Go.

```yaml
geth:
  after: [go]
  source: .
  plugin: go
  go-importpath: github.com/ethereum/go-ethereum
```

The `go` part specifies no plugin, which means it will be fetched from the parts repository. This is a collection of community-contributed definitions which can be used by anyone when building a snap, saving you from needing to specify the source and build rules for each system dependency. You can use `snapcraft search` to find more parts to use and `snapcraft define <part-name>` to verify how the part is defined.

In this case the go part requires only that we specify what version of Go we wish to build with for geth. As this part is not '`after`' anything else, it will be done first.

```yaml
go:
  source-tag: go1.7.5
```

#### Commands

Unlike the httplab example, geth contains multiple commands.

By default geth stores the blockchain in the home directory. Inside a snap this would be mapped to `$SNAP_USER_DATA`, the `~/snap/geth/current` directory. This directory is versioned. When the snap is updated the directory contents will be copied to a new revision, for example `~/snap/geth/1` to `~/snap/geth/2`. As the blockchain data can be quite large, we specify a `datadir` under `$SNAP_USER_COMMON`, the unversioned `~/snap/geth/common` directory.

```yaml
apps:
  abigen:
    command: bin/abigen
  bootnode:
    command: bin/bootnode
  evm:
    command: bin/evm
  faucet:
    command: bin/faucet
  geth:
    command: bin/geth --datadir "$SNAP_USER_COMMON/ethereum"
  puppeth:
    command: bin/puppeth
  rlpdump:
    command: bin/rlpdump
  swarm:
    command: bin/swarm
  wnode:
    command: bin/wnode
```

### Building the snap

You can build this example yourself by running the following:

```
git clone https://github.com/snapcraft-docs/go-ethereum
cd go-ethereum
snapcraft
```

The resulting snap can be installed locally. This requires the `--dangerous` flag because the snap is not signed by the Snap Store. The `--devmode` flag acknowledges that you are installing an unconfined application:

```
sudo snap install geth_*.snap --devmode --dangerous
```

Run the command:

```
geth account new
```

Removing the snap is simple too:

```
sudo snap remove geth
```

## Share with your friends

To share your snaps you need to publish them in the Snap Store. First, create an account on [the dashboard](https://dashboard.snapcraft.io/openid/login/?next=/dev/snaps/). Here you can customize how your snaps are presented, review your uploads and control publishing.

You’ll need to choose a unique “developer namespace” as part of the account creation process. This name will be visible by users and associated with your published snaps.

Make sure the `snapcraft` command is authenticated using the email address attached to your Snap Store account:

```
snapcraft login
```

### Reserve a name for your snap

You can publish your own version of a snap, provided you do so under a name you have registered.

```
snapcraft register mygosnap
```

Be sure to update the `name:` in your `snapcraft.yaml` to match this registered name, then run `snapcraft` again.

### Upload your snap

Use snapcraft to push the snap to the Snap Store.

```
snapcraft push --release=edge mygosnap_*.snap
```

If you’re happy with the result, you can commit the snapcraft.yaml to your GitHub repo and [turn on automatic builds](https://build.snapcraft.io) so any further commits automatically get released to edge, without requiring you to manually build locally.

## Further customisations

Here are all the Go plugin-specific keywords:

```
    - go-packages:
      (list of strings)
      Go packages to fetch, these must be a "main" package. Dependencies
      are pulled in automatically by `go get`.
      Packages that are not "main" will not cause an error, but would
      not be useful either.
      If the package is a part of the go-importpath the local package
      corresponding to those sources will be used.

    - go-importpath:
      (string)
      This entry tells the checked out `source` to live within a certain path
      within `GOPATH`.
      This is not needed and does not affect `go-packages`.

    - go-buildtags:
      (list of strings)
      Tags to use during the go build. Default is not to use any build tags.
```

You can view them locally by running:

```
snapcraft help go
```

### Extending and overriding behaviour

You can [extend the behaviour](/build-snaps/scriptlets) of any part in your `snapcraft.yaml` with shell commands. These can be run after pulling the source code but before building by using the `prepare` keyword. The build process can be overridden entirely using the `build` keyword and shell commands. The `install` keyword is used to run shell commands after building your code, useful for making post build modifications such as relocating build assets.

Using the geth example above, we can run the test suite at the end of the build. If this fails, the snap creation will be terminated:

```yaml
parts:
  go:
    source-tag: go1.7.5
  geth:
    after: [go]
    source: .
    plugin: go
    go-importpath: github.com/ethereum/go-ethereum
    install: |
      make test
```

<!--
## Next steps

Congratulations, you have an app in edge ready to share with other developers.

Want to learn more? Continue on to learn how to get your app ready for a wider audience.
-->
