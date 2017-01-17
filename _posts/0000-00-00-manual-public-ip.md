---
layout: page
title: "Add a public IP to a server"
category: use
order: 200
date: 2016-07-28 11:54:00
---


### Extra: Add public IP to an existing machine

#### 1) Request Public IP using jFed

Create a jFed experiment with the "Address Pool" resource. Give it an arbitrary name and run the experiment. After startin the experiment, you can view the public ip by right-clicking the pool and choosing "properties". Use this IP for the next steps.

#### 2) Give public IP to MAAS server

First, find out the name of the interface that is connected to the MAAS network. Ssh to the server and run `ifconfig`.

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
    bridge_ports enp1s0f0.28
```
