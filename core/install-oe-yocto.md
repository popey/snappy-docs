---
layout: base
title: Install snapd on OpenEmbedded/Yocto
---

As OpenEmbedded/Yocto is a meta distribution build system which doesn't provide
you by default with any binary packages, we don't do this either for snapd.

To allow inclusion of snapd in any Linux distribution built with
OpenEmbedded/Yocto, a so called meta-layer
[meta-snappy](https://github.com/morphis/meta-snappy/) exists which include all
necessary recipes. These recipes cover required components and define
dependencies on other dependencies outside of the meta-snappy layer.

To keep things simple we only show here how you can include meta-snappy in a
generic Yocto build targeting the QEMU emulator. More information about how to
start with Yocto can be found in the [Yocto Project Quick Start](https://www.yoctoproject.org/docs/2.4/yocto-project-qs/yocto-project-qs.html) manual.

First, setup the Yocto build environment based on the 2.4 (rocko) release:

```
git clone -b rocko git://git.yoctoproject.org/poky
```

Now we need to add the meta-snappy layer into the build environment:

```
cd poky
git clone https://github.com/morphis/meta-snappy.git
```

Snaps are delivered as squashfs archive files. Support for squashfs is provided
by meta-openembedded layer:

```
cd poky
git clone -b rocko git://git.openembedded.org/meta-openembedded
```

After this we can start setting up the actual build environment:

```
source oe-init-build-env
```

Once that is done, we need to add the meta-snappy and meta-openembedded layers
in the just created `conf/bblayers.conf` configuration file. Adjust the file to
look like the following, but replace `/path/to` with the correct absolute path
to the repositories we just cloned:

```
[...]
BBLAYERS ?= " \
   ...
   /path/to/meta-snappy \
   /path/to/meta-openembedded/meta-oe \
   /path/to/meta-openembedded/meta-filesystems \
"
```

Enable support for systemd which is mandatory for snapd. See the corresponding
section in [Yocto Development Tasks Manual](https://www.yoctoproject.org/docs/latest/dev-manual/dev-manual.html#using-systemd-exclusively)
for more details.

```
cat << EOF >> conf/local.conf
DISTRO_FEATURES_append = " systemd"
VIRTUAL-RUNTIME_init_manager = "systemd"
DISTRO_FEATURES_BACKFILL_CONSIDERED = "sysvinit"
VIRTUAL-RUNTIME_initscripts = ""
EOF
```

The `snap-confine` tool assumes that the home directory of `root` user is
`/root`. Make sure we do not break this assumption, otherwise snaps mount
namespace setup will fail early in the process. To use `/root', set `ROOT_HOME`
like this:`

```
cat <<EOF >> conf/local.conf
ROOT_HOME = "/root"
EOF
```

(Optional) The build can take up a huge amount of disk space, inheriting
`rm_work` class will help deal with that (see the [Yocto Reference Manual](https://www.yoctoproject.org/docs/latest/ref-manual/ref-manual.html#ref-classes-rm-work) for details). To remove unused 
build files add the following to `conf/local.conf`:

```
INHERIT += "rm_work"
# exclude snapd in case you want to develop snapd recipes
RM_WORK_EXCLUDE += "snapd"
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
