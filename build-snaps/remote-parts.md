---
layout: base
title: Remote parts
---

Developers using snapcraft to build snaps can leverage pre-existing 'remote parts' in their snap, to save time re-implementing existing components.

Developers who have created local parts which may be of use to others, can submit them to the remote part cache for other developer to consume. 

## Consuming remote parts

### Search for parts

The Snapcraft command line tool can search the remote parts cache using the `search` command. Before issuing `search` use the `update` command to update the parts list from the remote cache.

```
$ snapcraft update
Downloading parts list
```

Once updated, `search` on its own will return the entire list of remote parts, you can specify a search term to restrict the results returned.

```
$ snapcraft search curl
PART NAME  DESCRIPTION
curl       A tool and a library (usable from many languages) for client side URL transfers, supporting FTP, FTPS, HTTP, HTTPS, TELNET, DICT, FILE and LDAP.
```

### Viewing remote part contents

The remote part definition can be viewed using the `define` command in snapcraft. This is useful for checking the contents of a remote part before incorporating it into your build.

```
$ snapcraft define curl
Maintainer: 'Sergio Schvezov <sergio.schvezov@ubuntu.com>'
Description: A tool and a library (usable from many languages) for client side URL transfers, supporting FTP, FTPS, HTTP, HTTPS, TELNET, DICT, FILE and LDAP.

curl:
  configflags:
  - --enable-static
  - --enable-shared
  - --disable-manual
  plugin: autotools
  snap:
  - -bin
  - -lib/*.a
  - -lib/pkgconfig
  - -lib/*.la
  - -include
  - -share
  source: http://curl.haxx.se/download/curl-7.44.0.tar.bz2
  source-type: tar
```

### Using remote parts

There are a few ways to consume remote parts.

#### Implicit use

The simplest use of a remote part is to specify it in the `after` stanza for an existing part. This will consume the cached definition of the remote part as the snap is built.

```
parts:
    client:
       plugin: autotools
       source: .
       after: [curl]
```

#### Composing

Perhaps a remote part is almost the one that you need, but a change is required to fit your snap. In this case we can override pieces of the remote part. In this example we're overriding the source URL.

```
parts:
    client:
        plugin: autotools
        source: .
        after: [curl]
    curl:
        source: http://curl.haxx.se/download/curl-7.45.0.tar.bz2
```

#### Copy/Pasting

In this example we take the output from `snapcraft define <part>` and paste it directly into our snap. This allows us full control over all the pieces of the part. We are now effectively no longer using the remote part, but have incorporated it into our build definition.

```
parts:
    client:
        plugin: autotools
        source: .
        after: [curl]
    curl:
        configflags:
            - --enable-static
            - --enable-shared
            - --disable-manual
        plugin: autotools
        snap:
            - -bin
            - -lib/*.a
            - -lib/pkgconfig
            - -lib/*.la
            - -include
            - -share
        source: http://curl.haxx.se/download/curl-7.44.0.tar.bz2
        source-type: tar
```

## Creating remote parts

If you've created a part which might be useful for other developers, it's possible to share them easily.

### Host part

Create a repo containing only the part you wish to share. For example [https://github.com/sergiusens/curl](https://github.com/sergiusens/curl) contains the curl part mentioned above.

### Update parts wiki

Add a YAML formatted entry to the [parts wiki](https://wiki.ubuntu.com/snapcraft/parts) page. 

For example the `curl` part is defined thus.

```
---
origin: https://github.com/sergiusens/curl.git
maintainer: Sergio Schvezov <sergio.schvezov@ubuntu.com>
description:
  A tool and a library (usable from many languages) for
  client side URL transfers, supporting FTP, FTPS, HTTP,
  HTTPS, TELNET, DICT, FILE and LDAP.
parts: [curl]
---
```

*Note:* To edit the Ubuntu wiki you'll need an [Ubuntu SSO](https://login.ubuntu.com/) account (as used in the [snap store](https://dashboard.snapcraft.io/)), and need to request to join the [ubuntu-wiki-editors](https://launchpad.net/~ubuntu-wiki-editors) team. Once approved, logout from the Ubuntu wiki and log back in again to refresh your new credentials.

### Wait for cache refresh

The online parts cache refreshes from the wiki every 30 minutes. You can check the status (including time of most recent update) of the parts cache at [https://parts.snapcraft.io/v1/status](https://parts.snapcraft.io/v1/status).

### Promote your new part

Consider starting a thread on the [forum](https://forum.snapcraft.io/) to request feedback on, and promote the use of your new remote part. 
