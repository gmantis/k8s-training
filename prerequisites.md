# Kubernetes and Helm - Course Prerequisites

> Source: https://abinitio.atlassian.net/wiki/spaces/CIG/pages/1052186562/Kubernetes+and+Helm+-+Course+Prerequisites
> Last Modified: Feb 04, 2025 | Author: Tom Wren

# Machine Setup

The course will use your Ab Initio laptop or desktop. Those in IC can remote into their desktop from a slim terminal in the training room.

## Visual Studio Code

Install Visual Studio Code on your machine, available from here:

> `\\inode5.abinitio.com\installable\Microsoft\Visual Studio Code`

### Install these VSC extensions

#### "Remote Development" VSC Extension

This allows us to connect VSC to our VM and edit files, run a terminal etc.

Download via Extensions menu

### Create a private public ssh key pair

We'll use ssh keys to connect to the GCP compute instance that we'll do all the work on. You'll need to create this key pair before the course and send the public key to your instructor ahead of the start day.

Putty keys (.ppk) do not work directly, since VSCode uses openSSH, but working ssh keys can be generated using the Windows ssh-keygen command. Below is an example for the Ansible course, but you can do exactly the same for this course. Name the key as:

`<your-username>-helm-workshop`

To create the key run this on a **linux** machine. If you need to run this on a windows machine do not include the `-N ""` flag:

```
ssh-keygen -q -f ~/.ssh/$USER-helm-workshop -N ""
```

**Keep both files, send the .pub file to the course instructor**. They will add this to the GCP compute instance we will be using.

---

*Note: You may need to modify the Windows security settings for your newly-created private key to make it work — not just chmod of the file in ksh, but via Windows file properties. Remove group access rights that are there by default, and only leave your user's access (same idea as chmod, permissions of the file set to User only).*

---

## Wait for instructor to tell you your key has been added

## VScode remote connection setup

Once the command line connection works, setup your VScode remote connection like so:

* Press `Ctrl+Shift+P` to open the command palette
* Select "Remote-SSH: Open SSH Configuration File" and open the one in your windows user directory
* Add the following to the file:

```
Host workshop-controller
    HostName workshop-controller.cloudguild.gcp.abinitio.com
    User <YOUR USERNAME HERE>
    IdentityFile C:\Users\<YOUR USERNAME HERE>\.ssh\<YOUR USERNAME HERE>-helm-workshop
```

* Now test that VScode can open that connection by using `Ctrl+Shift+P` again but choose "Connect to Host" and select the "workshop-controller" host
* When that opens a new window, select **Linux** when prompted
* Also accept the ssh fingerprint when prompted

You now have the required connectivity for the class. With the remote window open, now install the Jupyter extension on the remote host by selecting "Install in SSH: workshop-controller".

## Workshop setup is now complete
