---
layout: page
title: "CI Pipeline"
category: dev
date: 2016-07-28 11:54:00
---
# Tengu CI pipeline

The [Tengu Charms repository](https://github.com/IBCNServices/tengu-charms) is connected to a CI pipeline that ends with publishing the Charms to the Juju Charms store.

Latest results from the following bundles:

- **[Spark](http://193.190.127.184:9080/spark/latest.html)**
- **[Storm](http://193.190.127.184:9080/storm/latest.html)**
- **[Sojobo](http://193.190.127.184:9080/sojobo/latest.html)**

Latest releases of charms and bundles are in [the Charm Store](https://jujucharms.com/u/tengu-bot/).

## High-level steps

This pipeline starts after a commit is pushed to the `tengu-charms` repository.

1. Push Charms and Bundles to the `unpublished` channel in the Charm store.
2. Spin up one Hauchiwa for every bundle to test.
3. Deploy the bundle using the Hauchiwa REST API.
4. Ssh into the Hauchiwa and test the bundle (using `cloud-weather-report`).
5. Publish test results on the [the Tengu CI website](http://193.190.127.184:9080/). The latest test results are available at `/<bundle-name>/latest.html`.
6. If tests succeed, Publish the Charms and bundles to the [`tengu-bot` personal namespace](https://jujucharms.com/u/tengu-bot/) in the Charm store.

# Under the hood

The CI pipeline is started by the [Tengu Jenkins](http://193.190.127.184:8088/). Jenkins calls the [`ci.sh` script](https://github.com/IBCNServices/tengu-charms/blob/master/ci.sh). The actual heavy lifting is done by the [`cihelpers.py` script](https://github.com/IBCNServices/tengu-charms/blob/master/cihelpers.py).
