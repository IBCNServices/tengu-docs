---
layout: page
title: "Acces your Applications"
category: use
order: 1
date: 2016-07-28 11:54:00
---

Do you want to have access to the applications running in your model? There are a couple of options.

## Use the VPN

The VPN gives you access to the internal network and DNS server. Both the VMWare and the MAAS cluster have two distinct VPN. This is the safest way to connect to a machine on the cluster. Make sure to use the right one for the right cluster! If you want to connect other servers or computers to the VPN, ask the Tengu team for the config file. More information on how to configure clients to connect to the VPN can be found in the [OpenVPN Charm README](https://github.com/IBCNServices/layer-openvpn#connecting-to-the-vpn).

*Note: If you want to give external (non-IDLab) parties access to the VPN, please ask the Tengu team for a custom config. This is so that we can revoke access when the project ends. **Do not give external parties your vpn config!**

- MAAS VPN gives you access to the `172.28.0.0/14` network and the `.maas` DNS domain (hostnames like `bland-lamp.maas` and `juju-35dfg5e-23-lxd-1`).
- VMWare VPN gives you access to the `10.10.136.0 255.255.252.0` network and the `.tenguvmware` DNS domain (hostnames `juju-0a40b7-0.tenguvmware`).

Once connected to the VPN, you can SSH to all servers. You can use both [juju ssh](https://jujucharms.com/docs/master/charms-working-with-units#the-juju-ssh-command) and normal ssh. Make sure to first [add your ssh key](https://jujucharms.com/docs/2.0/commands#add-ssh-key) before ssh-ing directly to the machines.

## Give the machine a public IP

If you have a machine with a publicly accessible IP in your experiment (193.190 range), you can either deploy your applications directly to that machine, or (preferred) deploy your applications into containers and set up a reverse proxy on the machine itself. Use an HTTP reverseproxy for HTTP webservices or use a TCP/UDP forwarder to forward all traffic.

If you don't have a machine with a public IP, you can [manually add it yourself](manual-public-ip).

## Use a proxy with a public IP

Instead of giving each machine that needs to be accessible a different public ip, you can install a proxy on one machine with a public IP and use that to proxy external requests to the correct service. Both the [HAProxy](https://jujucharms.com/haproxy) and the [Apache2](https://jujucharms.com/apache2) Charm support the "reverseproxy" relationship. Deploy the reverse proxy directly on a machine with a public ip and connect the reverse proxy to your application. For more information on this, see the README's of the [HAProxy](https://jujucharms.com/haproxy) and [Apache2](https://jujucharms.com/apache2) Charms.

*Note: This will only work for Charmed webservices that have the `http` relationship*

The [network-agent Charm](https://jujucharms.com/u/tengu-team/network-agent) can pose as a layer 2 (tcp and udp) forwarder. For more information, look at the network-agent README.
