---
layout: page
title: "Getting Started with Eclipse Che"
category: use
order: 1
date: 2016-07-28 11:54:00
---

[Eclipse Che](http://www.eclipse.org/che/) is a cloud IDE. Your browser becomes your IDE, your workspaces are docker containers. All the development tools, dependencies and libraries are already installed in the workspace. The only thing you have to do is surf to the url and start coding. To top it all off, you get an in-browser terminal right into your workspace.

**Use the in-browser IDE to develop applications.**

![IDE ](https://raw.githubusercontent.com/IBCNServices/layer-eclipse-che/master/files/create-project.gif)

**Use the full-featured in-browser commandline.**

![console view  ](https://raw.githubusercontent.com/IBCNServices/layer-eclipse-che/master/files/browser-commandline.gif)


# Create a Juju workspace

**Choose the Juju workspace.**
![Choose your stack view ](https://raw.githubusercontent.com/IBCNServices/layer-eclipse-che/master/files/create-workspace.gif)

Now wait until it is complete.

# Connect a Juju workspace to the `jujucharms.com` controller.

In the workspace, run the following commands in the terminal.

Run `juju add-credential` to add your AWS credentials.

```
[:] ubuntu@3feb084a1eab:/projects$ juju add-credential aws
Enter credential name: amazon
A credential with that name already exists.

Replace the existing credential? (y/N): y
Using auth-type "access-key".
Enter access-key: AFB03AABAEPAK3A5P6R
Enter secret-key:
Credentials added for cloud aws.
```

Run `juju register jimm.jujucharms.com --no-browser-login` to register the jujucharms.com controller.

```
[:] ubuntu@3feb084a1eab:/projects$ juju register jimm.jujucharms.com --no-browser-login
Login to Ubuntu SSO
Press return to select a default value.
E-Mail: merlijn.sebrechts@gmail.com
Password:
Two-factor auth (Enter for none):
Enter a name for this controller: jujucharms.com

Welcome, merlijn-sebrechts@external. You are now logged into "jujucharms.com".

```

List your existing models with `juju list-models` or add a new model with `juju add-model mymodel1 aws --credential amazon`

```
[jujucharms.com:] ubuntu@3feb084a1eab:/projects$ juju add-model mymodel1 aws --credential amazon
Uploading credential 'aws/merlijn-sebrechts@external/amazon' to controller
Added 'mymodel1' model on aws/ap-northeast-1 with credential 'amazon' for user 'merlijn-sebrechts'
[jujucharms.com:merlijn-sebrechts@external/mymodel1] ubuntu@3feb084a1eab:/projects$
```

Look at the Juju documentation for more info. The REAMDE's of the Charms also contain a bunch of information of how to use them.

- [How to ssh into units and copy files.](https://jujucharms.com/docs/2.1/charms-working-with-units)
- [Deploying Charms and managing applications.](https://jujucharms.com/docs/2.1/charms)
- [Viewing Logs.](https://jujucharms.com/docs/2.1/troubleshooting-logs)
- [Exposing Applications](https://jujucharms.com/docs/2.1/charms-exposing)
