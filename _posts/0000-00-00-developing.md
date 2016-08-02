---
layout: page
title: "CI Pipeline"
category: dev
date: 2016-07-28 11:54:00
---

# Testing on a Hauchiwa

It is advised that before every push, you run the tests locally. This is done in two steps:

#### 1. Deploy the bundle from your Hauchiwa

Copy the entire bundle folder to your Hauchiwa and run `tengu create`, supplying your bundle:

    tengu create stest1 --bundle ~/streaming/bundle.yaml


#### 2. Run the tests from your Hauchiwa

`cd` into the bundle dir, and run `bundletester`.

    ubuntu@h-merlijnt4:~/streaming$ bundletester
    cs:~tengu-bot/trusty/modelinfo-0
        charm-proof                                                            PASS
    cs:~tengu-bot/trusty/storm-2
        charm-proof                                                            PASS
        charm-proof                                                            PASS
    bundle
        charm-proof                                                            PASS
        00-setup                                                               PASS
        11-integration                                                         PASS

    PASS: 6 Total: 6 (103.728235 sec)
