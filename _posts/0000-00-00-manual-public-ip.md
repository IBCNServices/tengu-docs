---
layout: page
title: "Add a public IP to a server"
category: use
order: 200
date: 2016-07-28 11:54:00
---

## 1) Request Public IP using jFed

Create a jFed experiment with the "Address Pool" resource on the Virtual Wall 1. Give it an arbitrary name of max 8 characters and run the experiment.

*Protip: Make sure to set the expiration time long enough or you'll get conflicts from other people using the IP. The maximum expiration time is 90 days. If your machines need to be available longer, you'll need to manually renew it every few months. Put it in your calendar so you don't forget to renew it.*

After starting the experiment, you can view the public ip by right-clicking the pool and choosing "properties". Use this IP for the next steps.

*Protip: If you don't see the IP, close the experiment and open it again using the "recover" function.*

## 2) Assign the public IP to your machine

The instructions to assign the public IP to your machine differ depending on whether the machine is part of the VMWare cluster or the MAAS cluster.

### 2a) Give public IP to a VMWare Virtual Machine**

*Use these instructions to assign the public IP to a VMWare virtual machine. For MAAS, see below.*

Check if the interface `ens224` exists on the VM. If it does not exist, contact Sander (`sborny` on the IDLab Mattermost) to connect your VM to the DMZ and the interface will be added to the VM.

*Caution: adding a VM to the DMZ will require a full system reboot.*

```bash
ifconfig -a | grep -q ens224 && echo "interface ens224 exists!" || echo "interface ens224 NOT found"
```

The next steps depend on the Ubuntu version of your VM. Check via:
```bash
lsb_release -a
```

**Ubuntu version >= 18.04**

Create the file `/etc/netplan/60-tengu.yaml` and copy the following config, fill in the public ip from jFed.

```yaml
network:
  version: 2
  ethernets:
    ens224:
      dhcp4: no
      addresses: [<PUBLIC_JFED_IP>/26]
      gateway4: 193.190.127.129
```

Bring up the interface with `sudo netplan try`. If all goes well you should receive the following output. Press <kbd>Enter</kbd> to accept the new configuration. If you lose SSH connection to the machine after applying the config and you can not reconnect immediatly, wait 120 seconds for the changes to revert.

```bash
ubuntu@juju-37156e-28:/etc/netplan$ sudo netplan try
Warning: Stopping systemd-networkd.service, but it can still be activated by:
  systemd-networkd.socket
Do you want to keep these settings?


Press ENTER before the timeout to accept the new configuration


Changes will revert in 119 seconds
Configuration accepted.

# Check if the correct IP is being used
curl ipinfo.io/ip
```

This configuration will persist over reboots. It will take a while before Juju
shows the correct public-address. You can speed up this process by rebooting
the VM.

**Ubuntu version < 18.04**

Ensure that `ens224` is the name of the unconfigured network interface

```bash
ifconfig ens224
```

Edit the interfaces file `sudo nano /etc/network/interfaces` and add the following config at **the bottom** of that file.

```interfaces
auto ens224
  iface ens224 inet static
  address <the-public-ip-from-jfed>
  netmask 255.255.255.192
  pre-up route del default || true
  gateway 193.190.127.129
```

And bring up the interface with `sudo ifup ens224`.

```bash
# Bring up the public network interface
sudo ifup ens224
# Check if the correct IP is being used
curl ipinfo.io/ip
```

This configuration will persist over reboots. It will take a while before Juju
shows the correct public-address. You can speed up this process by rebooting
the VM.

### 2b) Give public IP to MAAS server

*Use these instructions to assign the public IP to a MAAS physical server. For VMWare, see above.*

First, find out the name of the interface that is connected to the MAAS network. Ssh to the server, and switch to the root user.

```bash
sudo su -
```

Run `ifconfig`.

```txt
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

```bash
PUBIP=<insert public ip>
BRIDGEIF=br-enp1s0f0

modprobe 8021q
vconfig add $BRIDGEIF 28
ifconfig $BRIDGEIF.28 $PUBIP netmask 255.255.255.192
route del default && route add default gw 193.190.127.129
```

And add the following lines to `/etc/network/interfaces`.

```interfaces
auto br-enp1s0f0.28
iface br-enp1s0f0.28 inet static
    up ip route del default || true
    up ip route add default via 193.190.127.129 || true
    address <insert public ip>/26
    vlan-raw-device br-enp1s0f0
```
