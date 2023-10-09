---
typora-copy-images-to: /assets/img
typora-root-url: ../

cover-img: /assets/logos/docker.png

lang: en-AU
layout: post
title: Relocating Docker Directory on Ubuntu
subtitle: 
author: Stephen
tags: [docker,ubuntu os,technical,2022,may]
comments: true
redirect_from:
 - /2022/05/20/relocating-docker-directory-on-ubuntu/
 - /2022/05/20/relocating-docker-directory-on-ubuntu
---

# Relocating Docker Directory on Ubuntu

Docker](https://www.docker.com/) is a way of containerising workloads and is useful either as a developer to enable you to speed up your development workflow; or as an end-user utilising [someone else’s container](https://hub.docker.com/) to run a workload on your environment. If someone has put in the work, then why would you re-invent the wheel?

Recently I needed to relocate my docker directory from its default location to a new path that had a better performance as it was impacting on speed of the blog loading, as it was just loading off the system disk, which only has one spindle.

I created a new raid-5 MD raid set of three spindles, allowing for additional HA (High Availability) in the case of one of the Hard Drives fails. This site is just being run on a Raspberry PI, so the USB disks are good quality (but still 2.5″ HDD).

This guide has been written for Ubuntu as that is what I am using, if you use a different distribution, you may need to change the source path.

This is the process that I used.

## Stop the Docker Service

```
sudo systemctl stop docker
```

## Edit the Docker daemon.json File

This file will be located in the path /etc/docker I used nano to edit the file.

```
sudo nano /etc/docker/daemon.json
```

Unless you have already added some configuration, this file will empty, so you must add the following.

```
{
  "data-root": "/path/to/your/docker"
}
```

## Use Clone the Original to the New Location.

You cannot just copy the directory structure, as we need to ensure that we keep all the file and directory permissions the same. You have two options:

* Move the files (best if on the same physical partition)
* Use Rsync (best if the files will not be on the same physical partition)

To move the files, use the following command

```
mv /var/lib/docker/ /path/to/your/docker
```

To use rsync use the following

```
sudo rsync -aP /var/lib/docker/ /path/to/your/docker
```

Once the file move/sync has been completed, we will rename the old location. Doing this will ensure that any errors with file paths will become immediately apparent.

```
mv /var/lib/docker /var/lib/docker.old
```

## Restart the Docker Service

Now it is time to restart the docker service and see if we missed anything.

```
sudo systemctl start docker
```

## Time to Test Everything!

Make sure that all your docker instances are running as expected, and then once you are happy, you can remove the docker.old directory. 

```
sudo rm -rf /var/lib/docker.old
```

## Create a Symlink to Help Others

I also created a symlink for /var/lib/docker to point to /path/to/your/docker this way you will be put in the right place when muscle memory takes over. It is also helpful to others that may work on the system that may not realise that the directory has been relocated.

```
sudo ln -s /path/to/your/docker /var/lib/docker
```