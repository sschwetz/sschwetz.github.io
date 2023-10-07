---
typora-copy-images-to: /assets/img
typora-root-url: ..

lang: en-AU
layout: post
title: Running avahi-daemon in LXC
subtitle: a quick howto
tags: [technical,systemd,avahi,2023,October]
comments: true
thumbnail: /assets/img/containers.png
cover-img: /assets/img/lxc-containers.jpg
---

For my compute, I prefer to run proxmox and it allows you to run containers via LXC. These will use the currently running kernel and boot close to instantly, with a much lower overhead than a KVM-based VM.

Anyhow, trying to run `avahi-daemon` in the containers often fails. I'm not the first to notice this, but the answers were unsatisfying until I found a suggestion to try running with `--no-rlimits`. 

To make the change persistence using `systemd`is simple.

1. Tell systemd that you wish to create an overide file
   ```
   systemctl edit avahi-daemon.service
   ```

2. once the system editor opens add the following to the top of the file
   ```
   [Service]
   ExecStart=
   ExecStart=/usr/sbin/avahi-daemon -s --no-rlimits
   ```

   
