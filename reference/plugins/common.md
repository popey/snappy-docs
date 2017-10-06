---
layout: base
title: Common keywords
---

There are several builtin keywords which can be used in any part regardless
of the choice of plugin.

  - `after`: [part, part, part...]

    Snapcraft will make sure that it builds all of the listed parts before
    it tries to build this part. Essentially these listed dependencies for
    this part, useful when the part needs a library or tool built by another
    part.

    If such a dependency part is not defined in this snapcraft.yaml, it must
    be defined in the cloud parts library, and snapcraft will retrieve the
    definition of the part from the cloud. In this way, a shared library of
    parts is available to every snap author - just say 'after' and list the
    parts you want that others have already defined.

  - `build-packages`: [deb, deb, deb...]

    A list of Ubuntu packages to install on the build host before building
    the part. The files from these packages typically will not go into the
    final snap unless they contain libraries that are direct dependencies of
    binaries within the snap (in which case they'll be discovered via `ldd`),
    or they are explicitly described in stage-packages.

  - `stage-packages`: [deb, deb, deb...]

    A list of Ubuntu packages to be downloaded and unpacked to join the part
    before it's built. Note that these packages are not installed on the host.
    Like the rest of the part, all files from these packages will make it into
    the final snap unless filtered out via the `snap` keyword.

  - `organize`: YAML

    Snapcraft will rename files according to this YAML sub-section. The
    content of the 'organize' section consists of old path keys, and their
    new values after the renaming.

    This can be used to avoid conflicts between parts that use the same
    name, or to map content from different parts into a common conventional
    file structure. For example:

        organize:
          usr/oldfilename: usr/newfilename
          usr/local/share/: usr/share/

    The key is the internal part filename, the value is the exposed filename
    that will be used during the staging process. You can rename whole
    subtrees of the part, or just specific files.

    Note that the path is relative (even though it is "usr/local") because
    it refers to content underneath parts/<part-name>/install which is going
    to be mapped into the stage and prime areas.

  - `filesets`: YAML

    When we map files into the stage and prime areas on the way to putting
    them into the snap, it is convenient to be able to refer to groups of
    files as well as individual files.  Snapcraft lets you name a fileset
    and then use it later for inclusion or exclusion of those files from the
    resulting snap.

    For example, consider man pages of header files.. You might want them
    in, or you might want to leave them out, but you definitely don't want
    to repeatedly have to list all of them either way.

    This section is thus a YAML map of fileset names (the keys) to a list of
    filenames. The list is built up by adding individual files or whole
    subdirectory paths (and all the files under that path) and wildcard
    globs, and then pruning from those paths.

    The wildcard * globs all files in that path. Exclusions are denoted by
    an initial `-`.

    For example you could add usr/local/* then remove usr/local/man/*:

        filesets:
          allbutman: [ usr/local/*, -usr/local/man/* ]
          manpages: [ usr/local/man ]

    Filenames are relative to the part install directory in
    `parts/<part-name>/install`. If you have used 'organize' to rename files
    then the filesets will be built up from the names after organization.

  - `stage`: YAML file and fileset list

    A list of files from a part install directory to copy into `stage/`.
    Rules applying to the list here are the same as those of filesets.
    Referencing of fileset keys is done with a $ prefixing the fileset key,
    which will expand with the value of such key.

    For example:

        stage:
          - usr/lib/*   # Everything under parts/<part-name>/install/usr/lib
          - -usr/lib/libtest.so   # Excludng libtest.so
          - $manpages                 # Including the 'manpages' fileset

  - `prime`: YAML file and fileset list

    A list of files from a part install directory to copy into `prime/`.
    This section takes exactly the same form as the 'stage' section  but the
    files identified here will go into the ultimate snap (because the
    `prime/` directory reflects the file structure of the snap with no
    extraneous content).

  - `prepare`: shell script

    If present, the shell script defined here is run before the `build` step
    of the plugin starts. The working directory is the base build
    directory for the given part. The defined script is run with `/bin/sh`.

    For example:

        prepare: |
          cd scripts
          ./bootstrap.sh

  - `build`: shell script

    If present, the shell script defined here is run instead of the `build`
    step of the plugin. The working directory is the base build directory
    for the given part. The defined script is run with `/bin/sh`.

    For example:

        plugin: make
        build: |
          make project
          make test
          make special-install

  - `install`: shell script

    If present, the shell script defined here is run after the `build` step
    of the plugin has finished. The working directory is the base build
    directory for the given part. The defined script is run with `/bin/sh`.

    For example:

        install: |
          sed -i 's|/usr/bin|$SNAP/usr/bin|g' my-bin-artifact.sh
          mv my-bin-artifact.sh $SNAPCRAFT_PART_INSTALL/bin/my-bin-build.sh
