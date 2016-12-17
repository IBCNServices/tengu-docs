---
layout: page
title: "External Acces to your Applications"
category: use
order: 1
date: 2016-07-28 11:54:00
---

Do you want to have access to the applications running in your model? There are a couple of options.

# Use the VPN

The tengumaas VPN gives you access to the internal network (`172.28.0.0/14` range). The VPN is already configured on your [Tengubox](tengubox). If you want to connect other servers or computers to the VPN, ask the Tengu team for the config file. More information on how to configure clients to connect to the VPN can be found in the [OpenVPN Charm README](https://github.com/IBCNServices/layer-openvpn#connecting-to-the-vpn).

*Note: If you want to give external (non-IDLab) parties access to the VPN, please ask the Tengu team for a custom config. This is so that we can revoke access when the project ends. **Do not give external parties your vpn config!***

## Get Console/SSH access to the applications

Once connected to the VPN, you can SSH to all servers. You can use [juju ssh](https://jujucharms.com/docs/master/charms-working-with-units#the-juju-ssh-command) on your tengubox or ssh directly to the machines. Make sure to first [add your ssh key](https://jujucharms.com/docs/2.0/commands#add-ssh-key) before ssh-ing directly to the machines.

## Get web access to the applications

When connected to the VPN, you can just surf to your applications. The VPN also enables the DNS server so hostnames like `bland-lamp.maas` and `juju-35dfg5e-23-lxd-1`.


# Expose HTTP webservices to the internet

Do you want applications to be available on the internet without the VPN? Both the [HAProxy](https://jujucharms.com/haproxy) and the [Apache2](https://jujucharms.com/apache2) Charm support the "reverseproxy" relationship. Deploy the reverse proxy directly on a machine with a public ip and connect the reverse proxy to your application. For more information on this, see the README's of the [HAProxy](https://jujucharms.com/haproxy) and [Apache2](https://jujucharms.com/apache2) Charms.

*Note: This will only work for Charmed webservices that have the `http` relationship*

# Expose TCP and UDP services to the internet

The [network-agent Charm](https://jujucharms.com/u/tengu-team/network-agent) can pose as a layer 2 (tcp and udp) forwarder. For more information, look at the network-agent README.
