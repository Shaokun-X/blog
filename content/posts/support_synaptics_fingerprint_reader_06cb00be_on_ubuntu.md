---
title: "Support Synaptics Fingerprint Reader 06cb:00be"
date: 2024-04-28T11:24:40+02:00
description: "Support Synaptics Fingerprint Reader 06cb:00be on Ubuntu"
tags: ["linux", "ubuntu", "driver", "fingerprint reader"]
draft: false
---

# A bit background

Lenovo did't release the official driver and `libfprint` could not support it [issue #296](https://gitlab.freedesktop.org/libfprint/libfprint/-/issues/296). However, luckily, there is a nice [GitHub repo](https://github.com/Popax21/synaTudor) that provides a viable solution.

# Let's go

Here I used Ubuntu, but other distros should be more or less the same while the package manager can be different, the naming of the deps might vary a little.

## Clone the project
```shell
git clone https://github.com/Popax21/synaTudor.git
```

## Install dependencies
```shell
# the tools if you don't already have
$ sudo install -y meson ninja cmake
# build deps
$ sudo install -y libgusb-dev libcap-dev libseccomp-dev libfprint-2-tod-dev libjson-glib-dev innoextract
# fprint
$ sudo install -y fprintd libpam-fprintd
```

## Build and install
```shell
cd synaTudor
meson build
cd build
ninja
sudo ninja install
```

Voilà, you should have the fingerprint option enabled in your desktop environment at this moment, or you can use fprint to manage manually:
```shell
fprintd-enroll -f right-index-finger USERNAME
fprintd-list USERNAME
fprintd-verify -f right-index-finger USERNAME
```