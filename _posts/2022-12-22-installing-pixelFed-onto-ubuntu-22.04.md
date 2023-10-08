---
typora-copy-images-to: /assets/img
typora-root-url: ../
cover-img: /assets/logos/mastodon.png

lang: en-AU
layout: post
title: Installing PixelFed onto Ubuntu 22.04
subtitle: 
author: stephen
tags: [fediverse,pixelfed,2022,December]
comments: true
redirect_from:
  - /stephen/installing-pixelfed-onto-ubuntu-22-04
---

Today I have been installing PixelFed onto a VM running Ubuntu 22.04 LTS, and for this, I used a couple of resources and some grey matter to get it working, so I better write this down before I forget.

This is designed more to be used by a technical resource to spin an instance up rather than an “end user”, and my thinking behind this is that it is still a beta. Initial ActivityPub looks to have been added in October, making it compatible the other Fediverse products like Mastodon.

## Update the Ubuntu OS

Once you have deployed the VM using the provider of your choice, I use [Binary Lane](https://binarylane.com.au/) for my VMs, you will need to upgrade them

```bash
sudo apt update
sudo apt upgrade -y
reboot now
```

## Install the Pre-Requistists

### Redis

```bash
apt -y install redis-server
systemctl enable redis-server
```

### **MariaDB**

If you have pre-existing MariaDB or MySQL Instance you can use this and change the below to suite

```bash
apt -y install mariadb-server
systemctl enable mariadb
mysql_secure_installation
mysql -u root -p
```

#### **Create the SQL Database**

```
create database pixelfed;
grant all privileges on pixelfed.* to 'pixelfed'@'localhost' identified by <yourpassword>';
flush privileges;
exit;
```

#### **Install PHP**

```bash
apt -y install php-fpm php-cli php-bcmath php-curl php-gd php-intl php-mbstring php-redis php-xml php-zip php-mysql
vi /etc/php/8.1/fpm/php.ini
```

#### Setup PHP-fpm

Set the following:

* upload_max_filesize to 100MB
* upload_max_filesize to 20-30 (how many files to a post
* max_execution_time to 600+ so upload tasks are not interrupted

#### **Install php-composer**

```bash
curl -sS https://getcomposer.org/installer -o /tmp/composer-setup.php
php /tmp/composer-setup.php --install-dir=/usr/local/bin --filename=composer
```

### Install Nginx and Certbot

```bash
apt -y install nginx certbot python3-certbot-nginx
systemctl enable nginx
certbot --nginx -d pixel.neurodiversity-in.au
```

#### **Install random prerequisites**

```bash
apt -y install ffmpeg jpegoptim optipng pngquant gifsicle unzip zip postfix
```

## **Install PixelFed**

```bash
mkdir -p /opt/pixelfed
cd /opt/pixelfed
git clone -b dev https://github.com/pixelfed/pixelfed.git pixelfed
composer install --no-ansi --no-interaction --optimize-autoloader
cp .env.example .env
```

### **Configure the environment file**

```bash
vi /opt/pixelfed/.env
```

Add / Edit the following lines to the .env config file

```
ENABLE_CONFIG_CACHE="true"
APP_NAME="Pixel | Neurodiversity"
INSTANCE_DESCRIPTION="Supporting Neurodiversity all the Way"
INSTANCE_DISCOVER_PUBLIC="true"
INSTANCE_CONTACT_EMAIL="<your email>
INSTANCE_PUBLIC_LOCAL_TIMELINE="true"
INSTANCE_PUBLIC_HASHTAGS="true"
APP_ENV="production"

# Database Configuration
DB_CONNECTION="mysql"
DB_HOST="db"
DB_PORT="3306"
DB_DATABASE="pixelfeed"
DB_USERNAME="pixelfeed"
DB_PASSWORD="<yourpassword>"

BACKUP_ARCHIVE_PASSWORD="<backup password>"
BACKUP_ARCHIVE_ADDRESS="<your email>"

OPEN_REGISTRATION="true"
MAX_ALBUM_LENGTH="30"

# ActivityPub Configuration
ACTIVITY_PUB="true"
AP_REMOTE_FOLLOW="true"
AP_INBOX="true"
AP_OUTBOX="true"
AP_SHAREDINBOX="true"
REMOTE_AVATARS="true"
NODE_INFO="true"
WEBFINGER="true"

AP_LOGGER_ENABLED="yes"
# Experimental Configuration# ActivityPub Configuration
ACTIVITY_PUB="true"
AP_REMOTE_FOLLOW="true"
AP_INBOX="true"
AP_OUTBOX="true"
AP_SHAREDINBOX="true"
REMOTE_AVATARS="true"
NODE_INFO="true"
WEBFINGER="true"
AP_LOGGER_ENABLED="yes"

## Mail Configuration (Post-Installer)
MAIL_DRIVER=sendmail
MAIL_HOST=127.0.0.1
MAIL_PORT=25
MAIL_USERNAME=null
MAIL_PASSWORD=null
MAIL_ENCRYPTION=null
MAIL_FROM_ADDRESS="pixelfed@<yourdomain>"
MAIL_FROM_NAME="Pixelfed Mailbot"

## S3 Configuration (Post-Installer)
PF_ENABLE_CLOUD="true"
FILESYSTEM_DRIVER="s3"
FILESYSTEM_CLOUD="s3"
AWS_ACCESS_KEY_ID="<key>"
AWS_SECRET_ACCESS_KEY="<secret>"
AWS_DEFAULT_REGION="<region>"
AWS_BUCKET="pixelfed"
AWS_URL="https://filecache.neurodiversity-in.au/pixelfed/"
AWS_ENDPOINT="https://filecache.neurodiversity-in.au"
AWS_USE_PATH_STYLE_ENDPOINT="true"

CUSTOM_EMOJI="true"
IMAGE_DRIVER="imagick"
INSTANCE_CONTACT_FORM="true"
INSTANCE_DISCOVER_PUBLIC="true"
INSTANCE_PUBLIC_LOCAL_TIMELINE="true"

STORIES_ENABLED="true"
LOG_CHANNEL="syslog"
LOG_LEVEL="DEBUG"
```

### **Change Ownership  on /opt/pixelfed**

```Bash
chown www-data:www-data /opt/pixelfed
```

### Run PHP Artisan Tasks

```bash
#One time only, you need to generate the secret APP_KEY:
php artisan key:generate

#One time only, the storage/ directory must be linked to the application:
php artisan storage:link

#Database migrations must be run:
php artisan migrate --force

#If you want to enable support for location data:
php artisan import:cities

#If you enabled ActivityPub federation:
php artisan instance:actor

#If you enabled OAuth:
php artisan passport:keys

#Routes should be cached whenever the source code changes or whenever you change routes:
php artisan route:cache
php artisan view:cache

#Every time you edit your .env file, you must run this command to have the changes take effect:
php artisan config:cache

### Laravel Horizon - Job queueing
php artisan horizon:install
php artisan horizon:publish
```

### **Create the system file**

```bash
tee /etc/systemd/system/pixelfedhorizon.service <<EOF
[Unit]
Description=Pixelfed task queueing via Laravel Horizon
After=network.target
Requires=mariadb
Requires=php7.4-fpm
Requires=redis
Requires=nginx

[Service]
Type=simple
ExecStart=/usr/bin/php artisan horizon --environment=production
ExecStop=/usr/bin/php artisan horizon:terminate --wait
User=pixelfed
WorkingDirectory=/opt/pixelfed
Restart=on-failure

KillSignal=SIGCONT
TimeoutStopSec=3600

[Install]
WantedBy=multi-user.target

EOF

systemctl daemon-reload
systemctl enable pixelfedhorizon
systemctl status pixelfedhorizon
```

#### **Crontab for schedule**

```bash
crontab -e
```

add this

```
* * * * * /usr/bin/php /home/pixelfed/pixelfed/artisan schedule:run >> /dev/null 2>&1
```

## **Configure Nginx**

```bash
cp /home/pixelfed/pixelfed/contrib/nginx.conf /etc/nginx/sites-available/pixelfed.conf
ln -s /etc/nginx/sites-available/pixelfed.conf /etc/nginx/sites-enabled/
nano /etc/nginx/sites-available/pixelfed.conf	
```

change:

* server_name pixel. (this appears twice)
* root /opt/pixelfed/public;
* ssl_certificate /etc/letsencrypt/live/pixel./fullchain.pem;
* ssl_certificate_key /etc/letsencrypt/live/pixel./privkey.pem;