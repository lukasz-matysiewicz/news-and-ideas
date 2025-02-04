---
ID: 62841
title: '0x2: Toolchain for Virtual Development Board'
author: piotr.krol
post_excerpt: ""
layout: post
permalink: >
  https://3mdeb.com/os-dev/0x2-toolchain-for-virtual-development-board/
published: true
date: 2013-06-07 08:42:00
archives: "2013"
tags:
  - embedded
  - linux
  - toolchain
  - virtual development board
  - VDB
categories:
  - OS Dev
  - App Dev
---
## Table of contents ##

* [Introduction](/2013/06/07/toolchain-for-virtual-development-board/#intro)
* [What is toolchain ?](/2013/06/07/toolchain-for-virtual-development-board/#what-is-toolchain)
* [Why we need cross-toolchain ?](/2013/06/07/toolchain-for-virtual-development-board/#why-we-need-cross-toolchain)
* [How to create toolchain ?](/2013/06/07/toolchain-for-virtual-development-board/#how-to-create-toolchain)
* [Emdebian path](/2013/06/07/toolchain-for-virtual-development-board/#emdebian-path)
* [Summary](/2013/06/07/toolchain-for-virtual-development-board/#summary)
* [Kudos](/2013/06/07/toolchain-for-virtual-development-board/#kudos)

<a id="intro"></a>
### Introduction ###

This is probably the most complicated topic of all related to embedded
development but we need to deal with it at the beginning. I read a lot about
toolchains but still don't know enough to explain details. I think that best
answers are in crosstool-ng [documentation](http://crosstool-ng.org/hg/crosstool-ng/file/0fc56e62cecf/docs).

<a id="what-is-toolchain"></a>
### What is toolchain ? ###

_Toolchain_ as the name said is a set of tools chained together, so output of
one tool is the input for different tool. This is well known concept in
Linux (eg. pipes). In embedded environment toolchain is called cross-toolchain or
cross-compiler, because usually it compiles on one architecture and generate
code for another (eg. it compiles on x86 and generate code for arm)[[1]](http://elinux.org/Toolchains).

<a id="why-we-need-cross-toolchain"></a>
### Why we need cross-toolchain ? ###

I suspect that your laptop/PC is not based on ARM processor, most probably it based on
x86 architecture so you cannot simply compile code and run it in our
virtual-arm-based environment. To prepare operating system and tools for it we
need cross-toolchain.

<a id="how-to-create-toolchain"></a>
### How to create toolchain ? ###

Process of creating cross-toolchain from scratch is not easy and takes some
time. There are few other ways to get toolchain, than creating it from scratch.
First we can use prebuilt toolchain providers like: `CodeSourcery`, `Linaro`, `DENX
EDLK` or `Emdebian`. Second we can create toolchain using special building system like: `Buildroot`, `Crosstool-NG`
or `Bitbake`. I will not deal with preparing toolchain in this series
because procedure for creating it takes pretty long. So we have two options:

* read my article about [Crosstool-NG arm-unknown-linux-gnueabi](/2013/04/03/yet-another-quick-build-of-arm-unknown-linux-gnueabi)
* or install toolchain ready to use like [Emdebian](http://www.emdebian.org/)

<a id="emdebian-path"></a>
#### Emdebian path ####

Add below lines to you `/etc/apt/sources.list`:
```bash
deb http://ftp.uk.debian.org/emdebian/toolchains unstable main
```
Install Emdebian keys, update and install cross-compiler with all related
packages:
```bash
sudo apt-get install emdebian-archive-keyring
sudo apt-get update
sudo apt-get install gcc-4.7-arm-linux-gnueabi
```

##### Dependency problems during installation #####
If above attempt to install cross-compiler ends with:
```
pietrushnic@eglarest:~/src$ sudo apt-get install gcc-4.7-arm-linux-gnueabi
Reading package lists... Done
Building dependency tree
Reading state information... Done
Some packages could not be installed. This may mean that you have
requested an impossible situation or if you are using the unstable
distribution that some required packages have not yet been created
or been moved out of Incoming.
The following information may help to resolve the situation:

The following packages have unmet dependencies:
 gcc-4.7-arm-linux-gnueabi : Depends: libgomp1-armel-cross (>= 4.7.2-5) but it
 is not going to be installed
 E: Unable to correct problems, you have held broken packages.
```
This means that Debian ustable cross-compiler is not available for you
configuration. You can read more about that
[here](http://lists.debian.org/debian-embedded/2011/05/msg00029.html).
To fix that issue simply change emdebian toochain repository to testing in
`/etc/apt/source.list`:
```
deb http://ftp.uk.debian.org/emdebian/toolchains testing main
```

##### Emdebian toolchain configuration #####
Check where `arm-linux-eabi-gcc-4.7` was installed:
```bash
whereis arm-linux-gnueabi-gcc-4.7
arm-linux-gnueabi-gcc-4: /usr/bin/arm-linux-gnueabi-gcc-4.7 /usr/bin/X11/arm-linux-gnueabi-gcc-4.7
```
It is not linked to `arm-linux-gnueabi-gcc`, so we cannot give its prefix as `CROSS_COMPILE`
variable value, which is needed for bootloader and kernel compilation. We have to link
it to `arm-linux-gnueabi-gcc`:
```bash
sudo ln -s /usr/bin/arm-linux-gnueabi-gcc-4.7 /usr/bin/arm-linux-gnueabi-gcc
```

Toolchain is ready to use.

_Note_: I tried `CodeSourcery` toolchain `arm-2012.09-64-arm-none-linux-gnueabi.bin`,
but it contain `binutils` defect that not allow correctly build kernel. If you see
something like this in log:
```
Error: selected processor does not support ARM mode `ldralt lr,[r1],#4'
```
That means you experience same thing, please use `Emdebian` or `Crosstool-NG` toolchain.

_Note 2_: If you're `Ubuntu` user I have to suggest experiments with toolchain
build by your own, because I get really hard times trying to go through this
tutorial with Ubuntu/Linaro cross compiler provided in repository. Finally I
used [this](/2013/04/03/yet-another-quick-build-of-arm-unknown-linux-gnueabi)
to push things forward. U-boot compiled with Ubuntu/Linaro toolchain had problem
with `__udivsi3` instruction. This cause loop in initialization process.

<a id="summary"></a>
### Summary ###
If you take effort of creating toolchain using `Crosstool-NG` than
congratulations.  But for simplifying whole [__Virtual Development Board__](/2013/06/07/intro-to-virtual-development-board-building)
series I will use `Emdebian` toolchain in further posts. Of course you can use
your brand new `Crosstool-NG` toolchain by simply remember that tools prefixes are
different. `Emdebian` uses `arm-linux-gnueabi-` and `Crosstool-NG` was created with
`arm-unknown-linux-gnueabi-`. Replace one with another every time when needed.
In [next post](/2013/06/07/embedded-board-bootloader) we will deal with
bootloader.

<a id="kudos"></a>
### Kudos ###

[1] [Toolchains](http://elinux.org/Toolchains)
