---
layout: base
title: Debugging
---

Snapd System specific debugging relates to determining whether a snap works correctly when confined and connected to its interfaces.

## Confinement

By default snaps run in a restrictive sandbox to ensure that they can only access the system and other snaps in controlled ways. The `confinement` key is used in snapcraft.yaml to specify whether or not the snap is expected to work correctly when the specified interfaces are connected and the snap is confined. You specify `strict` to indicate the snap works properly when confined or `devmode` to indicate it only works properly when unconfined. If confinement is unspecified, the snap is assumed to work correctly when confined since you are expected to develop your snaps for running in the sandbox. You're only allowed to upload snaps to the stable channel when strict confinement is used.

At the start of development you'll probably want to ignore confinement while you define your snap's interfaces. To do this specify `confinement: devmode` in your YAML like so:

    name: hello-world-service
    confinement: devmode
    apps:
      hello-service:
        command: command-hello-service.wrapper
        daemon: simple
        plugs:
        - network-bind
    ...

You then would specify `--devmode` when installing the snap for testing and development (you must always specify `--devmode` when installing in devmode regardless of how confinement is set in your yaml).

Once the application is working well in devmode, install it without specifying `--devmode` and iterate until it is working properly under confinement. When satisfied, you may indicate it works under confinement with:

    name: hello-world-service
    confinement: strict
    apps:
      hello-service:
        command: command-hello-service
        daemon: simple
        plugs:
        - network-bind
    ...

## Debugging confined apps

When debugging your snap when it's installed in `strict` mode (without `--devmode`), you can use the `grep "audit:" /var/log/syslog` command for sandbox denials. Alternatively, you may use the 'snappy-debug' snap to assist you:

    $ sudo snap install snappy-debug
    $ sudo /snap/bin/snappy-debug.security scanlog hello-world-service

This scanlog command will tail the syslog and make suggestions on what interfaces to use and suggest changes to make to your snap so it will work within the sandbox.