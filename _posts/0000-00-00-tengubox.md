---
layout: page
title: "Tengubox VM"
category: use
order: 4
date: 2016-07-28 11:54:00
---

# Introduction

Tengubox is a virtual machine with the necessary tools installed to administer your models and to develop Charms.

# Getting Started

1. Download and install [Oracle Virtualbox](https://www.virtualbox.org/wiki/Downloads)
2. Download and install the virtualbox "Extension Pac" to add support for USB.
2. Download the Tengubox VM and run it by opening the `Tengubox.vbox` file.
3. Create your Tengu user account, download `credentials.zip`, extract it and run `install_credentials.py`.
4. Switch to the Sojobo controller: `juju switch sojobo`
5. Login into the sojobo controller with `juju login` using your tengu user account.

Now everything is configured! Don't forget to turn on the tengu VPN when connecting to servers. To connect, click the network applet, and choose Tengumaas VPN from the VPN dropdown.

Possible next steps:

 - See existing models with `juju list-models` and switch to a model using `juju switch <modelanme>`.
 - Create a model active with `juju switch <modelname>` and show the status with `juju status`.
 - Add a new model with `juju add-model <new-modelname> --credential <username>`.
 - Go to Juju's UI with `juju ui`.
 - Switch between the Tengu models and your local models with `juju switch local` and `juju switch sojobo`.

# Local VS Sojobo

Tengu has a pool of physical machines that can be accessed using the Sojobo Controller. Use these machines when you need bare-metal performance. Your virtual machine can also deploy models locally into LXD containers. Spinning up local LXD containers is much faster and doesn't block resources so this is ideal for development purposes.

- Switch to the Local controller using `juju switch local`. LXD containers show up as "machines" in the local model. Deploying a service using `juju deploy xyz` will spin up a new container to deploy the service to.

- Switch to the Sojobo controler using `juju switch sojobo`. Deploying a service using `juju deploy xyz` will setup a new physical server and deploy the service to that server. You can also deploy services to LXD containers in existing services using `juju deploy xyz --to lxd:<machine#>`. It is advised to use containers as much as possible since the number of machines we have is limited.


