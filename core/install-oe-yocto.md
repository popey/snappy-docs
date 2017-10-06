---
layout: base
title: Install snapd on OpenEmbedded/Yocto
---

As OpenEmbedded/Yocto is a meta distribution build system which doesn't provide
you by default with any binary packages, we don't do this either for snapd.

To allow inclusion of snapd in any Linux distribution built with OpenEmbedded/Yocto, a so called meta-layer [meta-snappy](https://github.com/morphis/meta-snappy/)
exists which include all necessary recipes. These recipes cover required
components and define dependencies on other dependencies outside of the
meta-snappy layer.

To keep things simple we only show here how you can include meta-snappy in a
generic Yocto build targetting the QEMU emulator. More information about how
to start with Yocto can be found [here](https://www.yoctoproject.org/2.2/yocto-project-qs/yocto-project-qs.html).

First, setup the Yocto build environment based on the 2.2 (morty) release:

```
git clone -b morty git://git.yoctoproject.org/poky
```

Now we need to add the meta-snappy layer into the build environment:

```
cd poky
git clone https://github.com/morphis/meta-snappy.git
```

After this we can start setting up the actual build environment:

```
source oe-init-build-env
```

Once that is done, we need to add the meta-snappy layer in the just created
`conf/bblayers.conf` configuration file. Adjust the file to look like the follwoing, but replace `/path/to` with the correct absolute path to the meta-snappy repository
we just cloned:

```
[...]
BBLAYERS ?= " \
   ...
   /path/to/meta-snappy \
"
```

The last thing we need to do now before we can start a build is to adjust our
`conf/local.conf` configuration file to enable a few features snapd depends on:

```
cat << EOF >> conf/local.conf
DISTRO_FEATURES_append = " systemd"
VIRTUAL-RUNTIME_init_manager = "systemd"
DISTRO_FEATURES_BACKFILL_CONSIDERED = "sysvinit"
VIRTUAL-RUNTIME_initscripts = ""
EOF
```

That's it. Now we can start the build of the demo image the meta-snappy layer
provides, which will setup everything correctly:

```
bitbake snapd-demo-image
```

The build can take a while, depending on your machine, as it's building a whole
distribution from scratch. Once the build is done you can load the created image
in QEMU with:

```
runqemu qemux86
```

For more information about OpenEmbedded/Yocto please have a look at the official
documentation [here](https://www.yoctoproject.org/documentation).

## Next Steps

 * [Using snaps](usage)
