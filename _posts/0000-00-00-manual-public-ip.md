---
layout: page
title: "Add a public IP to a server"
category: use
order: 200
date: 2016-07-28 11:54:00
---


### Extra: Add public IP to an existing machine

#### 1) Request Public IP using jFed

Create a jFed experiment with the "Address Pool" resource on the Virtual Wall 1. Give it an arbitrary name of max 8 characters and run the experiment.

*Protip: Make sure to set the expiration time long enough or you'll get conflicts from other people using the IP. The maximum expiration time is 90 days. If your machines need to be available longer, you'll need to manually renew it every few months. Put it in your calendar so you don't forget to renew it.*

After starting the experiment, you can view the public ip by right-clicking the pool and choosing "properties". Use this IP for the next steps.

*Protip: If you don't see the IP, close the experiment and open it again using the "recover" function.*

#### 2) Give public IP to MAAS server

First, find out the name of the interface that is connected to the MAAS network. Ssh to the server, and switch to the root user.

```
sudo su -
```

Run `ifconfig`.

```
ubuntu@light-lab:~$ ifconfig
br-enp1s0f0 Link encap:Ethernet  HWaddr ...
          inet addr:172.28.0.62  Bcast:172.28.255.255  Mask:255.255.0.0
          ...

enp1s0f0  Link encap:Ethernet  HWaddr ...
          UP BROADCAST RUNNING MULTICAST  MTU:1500  Metric:1
          RX packets:38290355 errors:0 dropped:51433 overruns:0 frame:0
          ...
```

The maas network address is `172.28.0.0 255.255.0.0` so `br-enp1s0f0` is connected to the MAAS network. This is the interface we're looking for.

```
PUBIP=<insert public ip>
BRIDGEIF=br-enp1s0f0

modprobe 8021q
vconfig add $BRIDGEIF 28
ifconfig $BRIDGEIF.28 $PUBIP netmask 255.255.255.192
route del default && route add default gw 193.190.127.129
```

And add the following lines to `/etc/network/interfaces`.

```
auto br-enp1s0f0.28
iface br-enp1s0f0.28 inet static
    up ip route del default || true
    up ip route add default via 193.190.127.129 || true
    address <insert public ip>/26
    vlan-raw-device br-enp1s0f0
```
