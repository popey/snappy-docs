---
layout: base
title: Build your first snap
---

## A single text file

Snapcraft uses a single text file to describe the entire build process for a snap: the `snapcraft.yaml` file.

To get started with a template, you can run `snapcraft init`.

It will create a new directory `snap` with a file named `snapcraft.yaml` in the current directory.
Let's have a look inside:

```yaml
$ cat snap/snapcraft.yaml
name: my-snap-name # you probably want to 'snapcraft register <name>'
version: '0.1' # just for humans, typically '1.2+git' or '1.3.2'
summary: Single-line elevator pitch for your amazing snap # 79 char long summary
description: |
  This is my-snap's description. You have a paragraph or two to tell the
  most important story about your snap. Keep it under 100 words though,
  we live in tweetspace and your description wants to look good in the Snap
  Store.

grade: devel # must be 'stable' to release into candidate/stable channels
confinement: devmode # use 'strict' once you have the right plugs and slots

parts:
  my-part:
    # See 'snapcraft plugins'
    plugin: nil
```
<br>
This is the only file needed in the directory. Snapcraft can use local source code or fetch everything needed to build the snap from remote sources as you will see in the following example.

## From an app to a snap

Your first snap is going to be a very straightforward one. We are going to snap `GNU Hello`, an app that prints "Hello, world!" on the command line.

Let's edit the `snapcraft.yaml` file. First, we need to set the [general metadata](/build-snaps/syntax) for the snap:

* its name
* the version of GNU Hello we are going to snap
* a one-line summary and a user friendly description
* `grade` and `confinement`, which inform the system if the snap is meant to be a stable release (`grade: stable` or `devel`) and if strict security policies should be enforced (`confinement: strict` or `devmode`).

```yaml
name: hello
version: "2.10"
summary: GNU Hello, the "hello world" snap
description: GNU Hello prints a friendly greeting.
grade: stable
confinement: strict
```

<br>
Then, you need to declare [apps and commands](/build-snaps/metadata) shipped in the snap. In this case, we have a single app (`hello`) with a single command (`hello`).

```yaml
apps:
  hello:
    command: hello
```

<br>
Now, you need to tell snapcraft where to find the hello source code and how to build it. This is done with the `parts` keyword, which are [building blocks](/build-snaps/parts) that compose your snap.

In this case, it's a single part, pulled from a remote tar.gz `source`, then built using the `autotools` plugin (which will run the usual autotools steps: `configure`, `make`, `make install`).

```yaml
parts:
  gnu-hello:
    plugin: autotools
    source: http://ftp.gnu.org/gnu/hello/hello-2.10.tar.gz
```
<br>
Let's have a look at our final file:

```yaml
name: hello
version: "2.10"
summary: GNU Hello, the "hello world" snap
description: GNU Hello prints a friendly greeting.
grade: stable
confinement: strict

apps:
  hello:
    command: hello

parts:
  gnu-hello:
    plugin: autotools
    source: http://ftp.gnu.org/gnu/hello/hello-2.10.tar.gz
```

<br>
## Run snapcraft

You can now build a snap out of your `snapcraft.yaml` by running `snapcraft` in the project directory:

```
$ snapcraft
Preparing to pull gnu-hello
Pulling gnu-hello
Downloading 'hello-2.10.tar.gz'[=======================] 100%
Preparing to build gnu-hello
Building gnu-hello
[...build...]
Staging gnu-hello
Priming gnu-hello
No libraries to exclude from this release
Snapping 'hello' ...
Snapped hello_2.10_amd64.snap
```
<br>
Snapcraft just pulled the source code, built and snapped it.

The snap is now available in your directory (along with new `parts`, `prime`, and `snap` directories that allow faster iterations between builds of the same project).

```bash
$ ls
hello_2.10_amd64.snap  parts  snap  prime
```
<br>
The name of the `.snap` file is based on information in the `snapcraft.yaml` (its name and version) and the architecture you're building on: `<name>_<version>_<arch>.snap`. Here, the snap was built on an amd64 machine, so the architecture used in the file name reflects that.

## Install the snap

Now that everything needed for the hello app to run has been packaged inside a snap. You can install it locally.

The  `--dangerous` flag needs to be used here because your snap has not been signed.

```bash
$ sudo snap install hello_2.10_*.snap --dangerous
hello 2.10 installed
```

<br>
Congratulations, you've just made and installed your first snap!

You can run the `hello` command to see it in action:

```bash
$ hello
Hello, World!
```
<br>
You can also use the `snap info` command to get more details about it:

```bash
$ snap info hello
name:      hello
summary:   "GNU Hello, the \"hello world\" snap"
publisher:
description: GNU Hello prints a friendly greeting
commands:
  - hello
tracking:  
installed: 2.10 (x1) 65kB -
refreshed: 2016-12-23 17:03:55 +0100 CET
```

<br>
## More tutorials

[Ubuntu Tutorials](https://tutorials.ubuntu.com/) are just like learning from pair programming except you can do it on your own.

Recommended snap tutorials to get started:

* [Basic snap usage](https://tutorials.ubuntu.com/tutorial/basic-snap-usage?utm_source=snapcraft.io&utm_medium=buildfirstsnap&utm_campaign=tutorials)
* [Create your first snap](https://tutorials.ubuntu.com/tutorial/create-your-first-snap?utm_source=snapcraft.io&utm_medium=buildfirstsnap&utm_campaign=tutorials)
* [Snap a nodejs service](https://tutorials.ubuntu.com/tutorial/build-a-nodejs-service?utm_source=snapcraft.io&utm_medium=buildfirstsnap&utm_campaign=tutorials)