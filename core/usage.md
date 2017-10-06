---
layout: base
title: Use the snap command
---

This is an overview of commonly used snap commands. You can find the full command reference [here](/reference/snap-command).

## Log in to the Snap Store

Snaps are normally installed from the Snap Store. You can interact with the Snap Store without signing-in, but signing-in offers a number of advantages. These advantages include the ability to access your private snaps and managing your snaps without requiring root on the device.

The Snap Store holds a collection of snaps for delivery to clouds, devices, and private infrastructures. You sign-in as follows, using your [Ubuntu One account](https://login.ubuntu.com/+login):

    $ sudo snap login me@myself.com
    Password: *********
    2-factor: ******
    Welcome!

When you are not logged in, most `snap` commands will require you to run them as root.

## Find snaps

The Snap Store contains both public and private snaps.

Anybody can publish a snap, but doing a Snap Store search will only find snaps that are published to the `stable` release channel (and therefore have been reviewed and judged to be of good quality -- so users can install them without concerns).

Searches look for matches in the snap name or description:

    $ snap find hello
    Name           Version  Developer  Notes  Summary
    hello          2.10     canonical  -      GNU Hello, the "hello world" snap
    hello-huge     1.0      noise      -      A really big snap
    hello-world    6.1      canonical  -      Hello world example

Community developer Brian Douglass is also maintaining an [online frontend](https://uappexplorer.com/apps?type=snappy&sort=-last_updated) to browse the Snap Store.

## Install snaps

You can install snaps using the snap name. Here is an example of installing GNU Hello from the Free Software Foundation:

    $ snap install hello

## Run snaps

Each snap might include multiple related commands, with a default command that has the same name as the snap itself. Additional commands are prefixed with the snap name:

    $ hello
    Hello, world!
    $ hello.universe
    Hello, universe!

Snaps can also install services that run in the background, such as web servers and daemons. Those will start automatically when the snap is installed. The `snap disable <snap name>` and `snap enable <snap name>` commands allow you to keep full control over snaps installed on your system, regardless of the way they are started.

## View details about installed snaps

To see a list of all snaps installed on a system use `snap list`. The list also provides information on the software version, revision number, developer, and any extra notes provided with the snap (such as whether the snap is in developer mode or not).

    $ snap list
    Name           Version               Rev  Developer  Notes
    hello          2.10                  26   canonical  -
    ubuntu-core    16.04+20160419.20-55  109  canonical  -
    webdm          0.17                  21   canonical  -

## Automated updates and channels

Snaps are updated automatically in the background to the latest version, every day. This can also be done manually using `snap refresh` for either all installed snaps or by specifying a particular snap to refresh.

You can also switch to another version of a snap by refreshing into another release channel, if the developer has published the snap in multiple channels.

Valid release channels are `stable`, `candidate`, `beta`, `edge` and their name reflects the development status of snaps they contain.

    $ snap refresh <snap name> --beta

## Developer mode

When you're developing a snap, you will probably want to run it without the strict security confinement that is expected of stable, published snaps. This is done by using the `--devmode` flag on installation.

You can even publish snaps that require `--devmode` to work, but they can only be published to the `beta` or `edge` [release channels](/reference/channels). Users need to install these versions using the `--devmode` flag as well.

    $ snap install flubber --beta
    error: cannot install "flubber": snap not found
    $ snap install flubber --beta --devmode
    Download snap "flubber" (12) from channel "beta"
    ...

Because of the risk installing an unconfined app creates, installing a snap in `devmode` is not recommended unless you trust its developer. As a safeguard, snaps installed using the `devmode` flag are not automatically refreshed and require the user to manually refresh them.

To know the right channel and confinement mode to request when installing a package, you can use the `snap info` command prior to installation:

    $ snap info flubber
    name:      flubber
    summary:   ...
    publisher: flubber-dev
    description: ...
    channels:
      beta:   1.3 (16)
      edge:   1.4 (17) devmode

## Make your own snaps

Snaps can be easily created with a helper tool called snapcraft. Jump to the [Build snaps](/build-snaps) section to get started.