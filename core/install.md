---
layout: base
title: Install snapd
---

snapd, the service you need to install to run and manage snaps, is available on many distributions. This page gives an overview
of which ones are supported and what their current status is. It also provides installation
instructions for each of these distributions.

## Installation instructions

 * [Arch Linux](install-arch-linux)
 * [Debian](install-debian)
 * [Fedora](install-fedora)
 * [Gentoo](install-gentoo)
 * [Linux Mint](install-linux-mint)
 * [Manjaro](install-manjaro)
 * [OpenEmbedded/Yocto](install-oe-yocto)
 * [openSUSE](install-opensuse)
 * [OpenWrt](install-openwrt)
 * [Solus](install-solus)
 * [Ubuntu](install-ubuntu)

## Support Overview

The following overview shows which version of snapd is available in each of the
listed distributions.

| Operating System    | Status      | Version | Notes                   |
| ------------------- |:-----------:| ------- | ----------------------- |
| Ubuntu 14.04 LTS    | Supported   | 2.23    |                         |
| Ubuntu 16.04 LTS    | Supported   | 2.23    |                         |
| Debian (testing)    | Supported   | 2.21    | _devmode_               |
| Debian (unstable)   | Supported   | 2.21    | _devmode_               |
| Fedora 24           | End of Life | 2.26.3  | _devmode_, _no-classic_ |
| Fedora 25           | Supported   | 2.28.5  | _devmode_, _no-classic_ |
| Fedora 26           | Supported   | 2.28.5  | _devmode_, _no-classic_ |
| Fedora 27           | Supported   | 2.28.5  | _devmode_, _no-classic_ |
| Fedora Rawhide      | Supported   | 2.28.5  | _devmode_, _no-classic_ |
| CentOS 7            | In progress | N/A     | _devmode_, _no-classic_ |
| RHEL 7.3            | Unsupported | N/A     | N/A                     |
| Arch Linux          | Outdated    | 2.21    | _devmode_, _no-classic_ |
| Gentoo              | Outdated    | 2.15    | _devmode_               |
| openSUSE Leap 42.2  | Unsupported | 2.23.5  | N/A                     |
| openSUSE Tumbleweed | Unsupported | 2.23.5  | N/A                     |
| Yocto               | Unsupported | 2.23.5  | _devmode_               |

_devmode_: confinement technology is not fully supported and all snaps are
installed in [development mode](/reference/confinement).

_no-classic_: because of distribution policy to remove the `/snap` directory, snaps
using [classic confinement](/reference/confinement) are not supported.
