---
images: ./assets/
typora-copy-images-to: ./assets/
lang: en-AU
layout: post
title: Upgrading to Mastodon 4.2
subtitle: a quick howto
tags: [technical,mastodon,2023,October]
comments: true
---

The mastodon documentation contains great information on how to upgrade your mastodon instance between one update and another, but if you want the quick and dirty for a basic setup, here is the process.

1. Connect to your server via ssh
   ```bash
   systemctl stop mastodon-*
   ```

2. switch to the mastodon user account
   ```bash
   su - mastodon -s /bin/bash
   cd ~/live
   ```

3. Checkout the new version and make sure that we are using the correct local ruby version is installed
   ````bash
   git fetch
   git checkout v4.2.0
   rbenv install $(cat .ruby-version) --verbose
   ````

4. Upgrade the environment
   ```bash
   bundle install
   RAILS_ENV=production bundle exec rails db:migrate
   rm -rf node_modules && yarn install
   yarn build:production
   RAILS_ENV=production bundle exec rails assets:precompile
   ```

5. Finally start all the mastodon services. You will need to exit from the mastodon service account
   ````bash
   sudo systemctl restart mastodon-sidekiq.service
   sudo systemctl restart mastodon-web.service
   sudo systemctl restart mastodon-streaming.service
   ````

6. Shortly you should be able to access your instance