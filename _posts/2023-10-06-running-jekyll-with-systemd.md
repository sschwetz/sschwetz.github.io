---
images: assets/img
typora-copy-images-to: ../assets/img
lang: en-AU
layout: post
title: Running Jekyll with systemd
subtitle: 
tags: [technical,jekyll,2023,October]
comments: true
thumbnail: /assets/img/jekyll-icon.png
cover-img: /assets/img/jeykll-logo.png
---

I have been moving my blog from writefreely to Jekyll the last couple of days, which is why it has been a bit quiet on the posting side. 

The move could have been better thought out and more manageable. I accidentally deleted the VM that had the MySQL database from writing freely, so it recovered all that I could from [archive.org's Wayback machine](https://wayback.archive.org).

Once I had recovered everything that I could (some of the pages were not on the Wayback machine), I decided that I would prefer to keep the maintenance of the pages simple and at the same time, I wanted also to utilise a git repository to ensure that I had a version-controlled backup that was someone else's problem to backup. I'm looking at you [Github](https://github.com).

I will perform a full writeup of my setup and workflow for adding pages etc at a later date.

The main thing that I had some issues with was how to get the systemd file configured correctly so it is here, as if I have to restore the box I will have a copy and maybe you will stumble across it as well. 

The file has to be saved into `/etc/systemd/system/jekyall.service`

```
[Unit]
Description=Jekyll service
After=syslog.target
After=network.target

[Service]
WorkingDirectory=/home/jekyll

# Name of the user account that is running the Jekyll server
User=jekyll

#Set this to be production or development 
#Environment="JEKYLL_ENV="development"
Environment="JEKYLL_ENV=production"
Environment="JEKYLL_LOG_LEVEL=info"
Type=simple
ExecStart=bundle exec jekyll serve --watch -l --lsi --port 4000 --host schwetz.au  --source /home/jekyll
Restart=always
StandardOutput=journal
StandardError=journal
SyslogIdentifier=jekyll

[Install]
WantedBy=multi-user.target
```

Once you have saved the file and updated the `EXECStart=bundle exec jekyll serve --watch -l --lsi --port 4000 --host schwetz.au  --source /home/jekyll` to match your requirements. 

Once you have saved the file you will need to get systemd to reload its daemon using the command `systemctl daemon-reload`.  You can then enable and start the jekyll process using `systemctl enable --now jekyll.service`

**One thing to note:** with my setup via a reverse proxy, my theme Beautiful Jekyll when using social sharing, was using the internal private IP address of the machine rather than the URL provided in the configuration. To get around this, I edited /etc/hosts and added the following line to it `10.6.61.32 	schwetz.au www.schwetz.au` this would allow the system to resolve the hostname to the internal IP of the LXC Container running the system.