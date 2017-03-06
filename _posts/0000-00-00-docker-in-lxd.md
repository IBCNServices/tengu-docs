---
layout: page
title: "Docker in LXD"
category: use
order: 1
date: 2016-07-28 11:54:00
---

This tutorial explains how to run docker in lxd containers using Juju.

Basically, to run a docker container inside an lxd container, you need to use the `docker` lxd profile. However, Juju always used the `default` profile for lxd containers. What you need to do is to add the docker profile to the lxd profile.

```bash
lxc profile edit default
```

And change it to the following profile.

```yaml
config:
  environment.http_proxy: http://[fe80::1%eth0]:13128
  user.network_mode: link-local
  linux.kernel_modules: overlay, nf_nat
  security.nesting: "true"
description: Default LXD profile
devices:
  eth0:
    name: eth0
    nictype: bridged
    parent: lxdbr0
    type: nic
  aadisable:
    path: /sys/module/apparmor/parameters/enabled
    source: /dev/null
    type: disk
name: default
```
