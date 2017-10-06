---
layout: base
title: What is a snap?
---

A `snap` :

* is a squashFS filesystem containing your app code and a `snap.yaml` file containing specific metadata. It has a read-only file-system and, once installed, a writable area.

* is self-contained. It bundles most of the libraries and runtimes it needs and can be updated and reverted without affecting the rest of the system.

* is confined from the OS and other apps through security mechanisms, but can exchange content and functions with other snaps according to fine-grained policies controlled by the user and the OS defaults.