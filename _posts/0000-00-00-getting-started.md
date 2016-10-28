---
layout: page
title: "Getting Started"
category: use
order: 1
date: 2016-07-28 11:54:00
---
## Create Virtual Wall account and join Tengu project

Tengu can setup experiments (models) on the iMinds Virtual Wall. To use this functionality, you need to [create a virtual wall account](https://authority.ilabt.iminds.be/) and request access to the `tengu` project.  

## Tengu web-ui

[Tengu's web-ui](http://tengu.intec.ugent.be/dev-ui/) allows you to deploy models on the iMinds virtual wall.

1. Open the UI in your browser and click both "connect" buttons.
2. Click **"select your member authority"** in the "GENI Authorization tool" popup and choose **wall2.ilabt.iminds.be**.
3. Click "download certificate" and login using your virtual wall account.
4. Confirm the certificate request in the popup.
5. Enter the passphrase of your virtual wall private key (should be the same as your virtual wall account password).

After logging in, you can use the web-ui to create models.

## The hauchiwa

Each model is managed by a Hauchiwa. You can manage the model manually by logging into the Hauchiwa using ssh (username: `ubuntu`).

From the Hauchiwa, you can see the real-time status of the model by running the following command.

    watch juju status --format tabular

*exit using <kbd>ctrl</kbd>-<kbd>c</kbd>*

### Get console access to the services

Ssh to a service by running the following command **from your hauchiwa**.

    # Run this from your hauchiwa
    juju ssh <service-name>/<unit-number>

Similarly, you can copy files using `juju scp`.

### Get web access to the services

All webservices connect to an internal network not accessible from the internet. You can connect to these services using the `openvpn` service. To connect, first copy the vpn to your hauchiwa.

    # Run this from your hauchiwa
    juju scp openvpn/0:~/client1.tgz .

Then copy the config to your laptop *(for example by using `scp` from your laptop).*

Install the [openvpn client](https://openvpn.net/index.php/open-source/downloads.html) and setup the vpn using the client config file. When connected to the VPN, you can surf to the services using their private ip address and port number (192.168.14.xxx:bbbb).

### Give a service a public IP

Tengu allows you to easily forward public ports to private services using the dhcp-server. For services managed by Tengu that state their open ports, you can easily setup a port forward by running.

    tengu expose-service <servicename>


For services not managed by Tengu or services that don't specify their open ports, you'll have to tell the dhcp-server to forward a port manually.

```bash
# Create a file to configure the port forwards
nano pf.yaml
```

Put the following configuration in the file. You can see the ip in `juju status --format tabular`. *Please note that this will overwrite the port-forward config of the dhcp server. If the dhcp-server already forwards ports and you want to keep those forwards, you need to include them in the config. You can see the current port-forward config of the dhcp-server by running  `juju get dhcp-server`.*

```yaml
dhcp-server:
  port-forwards: |
    [{
      "public_port": "<port>",
      "private_port": "<port>",
      "private_ip": "<ip>",
      "protocol": "<tcp|udp>"
    }]
```

Save the file (<kbd>ctrl</kbd>-<kbd>o</kbd> <kbd>enter</kbd> <kbd>ctrl</kbd>-<kbd>x</kbd>) and tell the dhcp server to update the port-forwards accordingly.

```bash
juju set dhcp-server --config pf.yaml
```

You can verify the change using `juju get dhcp-server`. Now find the public ip of your model by looking at the `juju status --format tabular` output. The ip address between brackets is the public ip of your model.

```
ID                 WORKLOAD-STATE AGENT-STATE MACHINE PORTS    PUBLIC-ADDRESS                 MESSAGE                                    
dhcp-server/0      active         idle        0                n063-14b.wall2.ilabt.iminds.be Ready (193.190.127.240)                        
```
