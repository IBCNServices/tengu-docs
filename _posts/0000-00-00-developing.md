---
layout: page
title: "Developing"
category: dev
date: 2016-07-28 11:54:00
---
# Developing on a Hauchiwa

If you set the `charm-repo-source` config option of a Hauchiwa to the charms repository (`https://github.com/IBCNServices/tengu-charms.git`), you the charms repository will be downloaded to `/opt/tengu-charms/` and the right environment variables will be set to use that repo as a local repo. (log out and back in to the hauchiwa for this to take effect).

You can build layered charms by running `charm build <charmname> --series trusty`.

# Testing on a Hauchiwa

It is advised that before every push, you run the tests locally. This is done in two steps:

#### 1. Deploy the bundle from your Hauchiwa

Copy the entire bundle folder to your Hauchiwa and run `tengu create`, supplying your bundle:

    tengu create stest1 --bundle ~/streaming/bundle.yaml

*If you want this bundle to deploy local charms, you will have to specify that in the bundle.*

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

# Git quickstart
```bash
# Configure git
git config --global user.name "My Name"
$ git config --global user.email your@email.com

# Show which files have changed and what is staged/untracked
git status

# Stage changes so they will be included in the next commit
git add charms/trusty/...
# Note: After removing a directory, you also have to add the changes with `git add dir/that/doesn't/exist/anymore`

# Create the commit
git commit -m "much change, so commit, wow!"

# Push the commit to Github
git push upstream master
```
