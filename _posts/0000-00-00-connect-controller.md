---
layout: page
title: "Connect to the VMWare controller"
category: advanced
order: 1
date: 2017-12-03 11:54:00
---

## Connect to the controller

Run the command you received and enter a password of your choosing.

```bash
# Run the command you received
juju register ME0TBnNib3JueTAUExIxMC4xMC4xMzkuNzQ6MTcwNzAEIJleL1YG9u5zDbUetT2YXJYGEfrTSLtMuwmngJReTPawEwt2bXdhcmUtbWFpbgAA
```

This gives you access to the controller and all the models that you are granted.

## Instructions for cluster admins

These instructions are for cluster administrators only. Cluster administrators can also create new models and manage existing virtual machines. Start with the instructions for normal users and then continue with these instructions.

Create a file `nano vmware1.yaml` and add the following content: (as-is, do no change anything).

```yaml
clouds:
  vmware1:
    type: vsphere
    auth-types: [userpass]
    endpoint: vcenter.ilabt.imec.be
    regions:
      ILABT:
        endpoint: vcenter.ilabt.imec.be
```

Add the cloud using that file:

```bash
juju add-cloud vmware1 vmware1.yaml
```

Create a new credential for that cloud. Make sure to use your intec password, not the password you just filled when you ran `juju register`.

```txt
$ juju add-credential vmware1
Enter credential name: vmware-credential
Using auth-type "userpass".
Enter user: <intec-username>@intec.ugent.be
Enter password: <intec password>
Credentials added for cloud vmware1.
```

Now you can create a model specifying the credential.

```bash
juju add-model <modelname> --controller vmware-main --credential vmware-credential
```
