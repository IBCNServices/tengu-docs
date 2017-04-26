---
layout: page
title: "Getting Started with Juju as a Service"
category: use
order: 1
date: 2016-07-28 11:54:00
---

# Table of Contents

1. [Create your first model](#step-1-create-your-first-model)
1. [Create a Juju workspace in Eclipse Che](#step-2-create-a-juju-workspace-in-eclipse-che)
2. [Connect your Juju workspace to your model](#step-3-connect-a-juju-workspace-to-the-jujucharmscom-controller)
3. [More Documentation](#look-at-the-juju-documentation-for-more-info)

# Step 1. Create your first model

Before we begin, make sure that you have credentials for either AWS, GCE or Azure. Juju will request virtual machine from that cloud to run your applications in.

1. Go to [https://jujucharms.com]() and click on "Login".
2. Login with your Ubuntu One account (launchpad account), or create one if you don't have it.
3. After login, go back to [https://jujucharms.com]() and you should be automatically redirected to your profile page. Click "Start Building".

This is the Juju GUI. You use it to create models and manage and monitor applications.

The Juju commandline tools, however, are a lot more powerful than the GUI so the first application we're going to use Eclipse Che, a cloud IDE that gives us access to the Juju CLI tools.

5. Search for "Eclipse Che" in the Juju GUI and add it to the model.
6. Click "Deploy Changes" to deploy this model and give the model a name.
7. Choose which cloud you want to deploy to and add your credentials for that cloud. You can also add an ssh key that will be put on all the machines Juju creates.
8. Click "deploy".

Juju is now requesting a virtual machine from your cloud and will install eclipse che onto that cloud. By default, Eclipse Che will not be publicly accessible. You can make it publicly accessible by "exposing" it. Click on eclipse che and expose it using the menu on the left. Now we wait until eclipse che finishes installation.

The circle will turn grey and the unit of Eclipse Che will show the status "Ready (eclipse/che)".-->

# What is Eclipse Che?

[Eclipse Che](http://www.eclipse.org/che/) is a cloud IDE. Your browser becomes your IDE, your workspaces are docker containers. All the development tools, dependencies and libraries are already installed in the workspace. The only thing you have to do is surf to the url and start coding. To top it all off, you get an in-browser terminal right into your workspace.

**Use the in-browser IDE to develop applications.**

![IDE ](https://raw.githubusercontent.com/IBCNServices/layer-eclipse-che/master/files/create-project.gif)

**Use the full-featured in-browser commandline.**

![console view  ](https://raw.githubusercontent.com/IBCNServices/layer-eclipse-che/master/files/browser-commandline.gif)


# Step 2. Create a Juju workspace in Eclipse Che

Go to your Eclipse Che instance.

**Choose the Juju workspace.**
![Choose your stack view ](https://raw.githubusercontent.com/IBCNServices/layer-eclipse-che/master/files/create-workspace.gif)

Now wait until it is complete.

# Step 3. Connect a Juju workspace to the `jujucharms.com` controller.

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

# Look at the Juju documentation for more info.

The README's of the Charms also contain a bunch of information of how to use them.

- [How to ssh into units and copy files.](https://jujucharms.com/docs/2.1/charms-working-with-units)
- [Deploying Charms and managing applications.](https://jujucharms.com/docs/2.1/charms)
- [Viewing Logs.](https://jujucharms.com/docs/2.1/troubleshooting-logs)
- [Exposing Applications](https://jujucharms.com/docs/2.1/charms-exposing)
