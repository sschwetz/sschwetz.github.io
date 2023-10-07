---
typora-copy-images-to: /assets/img
typora-root-url: ..

lang: en-AU
layout: post
title: Protecting Your Mastodon Instance with rapidblock
subtitle: Haters will be haters
tags: [writefreely,mastodon,technical,2023,january]
comments: true
redirect_from:
  - /stephen/protecting-your-mastodon-instance-with-rapidblock
---

The RapidBlock Project is a grassroots initiative to make Fediverse domain blocking more effective through collective action.

Moderation on the Fediverse is unevenly distributed. Some instance admins devotedly follow the #FediBlock hashtag, blocking abusive servers within hours of their first appearance on the network. Others wait until their own users file a report. Still others do nothing at all.

## Installation

Installation with an debian based Linux distribution is as easy as running

```bash
curl -s https://apt.rapidblock.org/install.sh | sudo bash
```

This will run through installing the apt repository, pulling the required packages, and installing them.

## Configuration

Configuration for the toolset is fairly simple and is done through a text file located at /etc/default/rapidblock by default the file looks like the below:

```
# vim:set ft=sh:
#
# This file contains the configuration settings for the RapidBlock cron script.
#
# In source control, this file lives at:
#   https://github.com/rapidblock-org/rapidblock/blob/main/dist/cron.default
#
# When installed via a package manager, this file lives at:
#   /etc/default/rapidblock
#
# The crontab file lives at:
#   /etc/cron.d/rapidblock
#
# The script itself lives at:
#   /opt/rapidblock/scripts/cron.sh

ENABLED=0
SLEEP_MIN=0
SLEEP_MAX=3600
BLOCKLIST_URL="https://rapidblock.org/blocklist.json"
SIGNATURE_URL="https://rapidblock.org/blocklist.json.sig"
PUBLIC_KEY_FILE="/opt/rapidblock/share/rapidblock-dot-org.pub"
INSTANCES=( \
  "mastodon-4.x|postgresql:///mastodon?host=/run/postgresql&port=5433" \
)
```

In this format, the toolset is disabled and needs configuration you need to set the following `ENABLED=1` set instance type and use either mastodon-3.x or mastodon-4.x matching the version of Mastodon you are using.

### For a local database:

```
INSTANCES=( \
  "mastodon-4.x|postgresql:///mastodon_database_name?host=/run/postgresql&port=5433" \
)
```

### For a remote database:

```text
INSTANCES=( \
  "mastodon-4.x|postgresql:///mastodon_database_name?host=db.host.name&port=5432&user=username&password=secret" \
)
```

Once these have been set the script will be called via a cronjob every hour where it will sleep a random amount of time from 0 to 3600 seconds and will then download the update and install it.