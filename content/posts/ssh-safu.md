---
title: "How to security store SSH keys"
date: 2020-02-06T03:20:54+02:00
draft: false
description: A relatively safe way to use SSH keys.
tags:
  - Linux
  - ssh
  - security
---

A relatively safe way to use SSH keys.

The fact is that storing keys in open form in the home directory a priori can't be safe. The standard password managers [kde wallet](https://wiki.archlinux.org/index.php/KDE_Wallet)  and [GNOME Keyring](https://ru.wikipedia.org/wiki/GNOME_Keyring) don't store ssh keys by the default and, more sadly, don't have an adequate interface for casual usage.



To solve the problem, I suggest using a wonderful utility: [KeePassXC](https://keepassxc.org). Yes, it’s three times a fork of a rather old program, but it is remarkably supported, translated to Qt5, has plug-ins for integration with various browsers and can be used as a secure storage for all possible data, including using TOTR (+ Steam Guard).

First, check if you have ssh-agent running:

```bash
❯ ps -fp $SSH_AGENT_PID
UID          PID    PPID  C STIME TTY          TIME CMD
georgiy      875       1  0 02:50 ?        00:00:00 ssh-agent -s

```

If not, add the ssh-agent autorun script to the end of the `~ / .bashrc` file:

```bash
SSH_ENV="$HOME/.ssh/environment"

function start_agent {
    echo "Initialising new SSH agent..."
    /usr/bin/ssh-agent | sed 's/^echo/#echo/' > "${SSH_ENV}"
    echo succeeded
    chmod 600 "${SSH_ENV}"
    . "${SSH_ENV}" > /dev/null
}

# Source SSH settings, if applicable
if [ -f "${SSH_ENV}" ]; then
    . "${SSH_ENV}" > /dev/null
    #ps ${SSH_AGENT_PID} doesn't work under cywgin
    ps -ef | grep ${SSH_AGENT_PID} | grep ssh-agent$ > /dev/null || {
        start_agent;
    }
else
    start_agent;
fi
```

Switch on SSH AGENT support in KeePassXC:

**Tools** --> **Settings** --> **SSH-Agent** --> **Enable SSH Agent**

![2020-02-06_03-41](/2020-02-06_03-41.png)



Next, add the SSH key to KeePassXC (if the key is password protected, specify a password for it):


![2020-02-06_03-46](/2020-02-06_03-46.png)



In the SSH Agent tab, add our SSH key, and note the settings for automatically importing the key when unlocking the KeePassXC storage:



![2020-02-06_03-50](/2020-02-06_03-50.png)


You can use the command `ssh-add -l` for checking if your key has loaded in ssh-agent.
