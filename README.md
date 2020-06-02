# Wordpress Docker image running on Alpine Linux

[![Docker Automated build](https://img.shields.io/docker/automated/canarysat/alpine-php-wordpress.svg?style=for-the-badge&logo=docker)](https://hub.docker.com/r/canarysat/alpine-php-wordpress/)
[![Docker Pulls](https://img.shields.io/docker/pulls/canarysat/alpine-php-wordpress.svg?style=for-the-badge&logo=docker)](https://hub.docker.com/r/canarysat/alpine-php-wordpress/)
[![Docker Stars](https://img.shields.io/docker/stars/canarysat/alpine-php-wordpress.svg?style=for-the-badge&logo=docker)](https://hub.docker.com/r/canarysat/alpine-php-wordpress/)

[![Alpine Version](https://img.shields.io/badge/Alpine%20version-v3.12.0-green.svg?style=for-the-badge)](http://alpinelinux.org/)
[![Wordpress Version](https://img.shields.io/badge/Wordpress%20version-vlatest-green.svg?style=for-the-badge)](https://www.wordpress.org/en/)



This Docker image [(canarysat/alpine-php-wordpress)](https://hub.docker.com/r/canarysat/alpine-php-wordpress/) is based on the minimal [Alpine Linux](http://alpinelinux.org/) ready for running [WordPress](https://www.wordpress.org/). (Requires external database)

##### Alpine Version 3.12.0 (Released May 31, 2020)
##### Wordpress Version latest
##### PHP Version 7.3.18
##### Nginx Version 1.16.1

----

## What is Alpine Linux?
Alpine Linux is a Linux distribution built around musl libc and BusyBox. The image is only 5 MB in size and has access to a package repository that is much more complete than other BusyBox based images. This makes Alpine Linux a great image base for utilities and even production applications. Read more about Alpine Linux here and you can see how their mantra fits in right at home with Docker images.

## What is Wordpress?
WordPress is an online, open source website creation tool written in PHP. But in non-geek speak, it's probably the easiest and most powerful blogging and website content management system (or CMS) in existence today.

## Features

* Minimal size only, minimal layers
* Memory usage is minimal on a simple install


## Architectures

* ```:amd64```, ```:x86_64``` - 64 bit Intel/AMD (x86_64/amd64)

##### PLEASE CHECK TAGS BELOW FOR SUPPORTED ARCHITECTURES, THE ABOVE IS A LIST OF EXPLANATION

## Tags

* ```:latest``` latest branch based (Automatic Architecture Selection)
* ```:master``` master branch usually inline with latest
* ```:amd64```, ```:x86_64```  amd64 based on latest tag but amd64 architecture

## Layers & Sizes

![Version](https://img.shields.io/badge/version-amd64-blue.svg?style=for-the-badge)
![MicroBadger Layers (tag)](https://img.shields.io/microbadger/layers/canarysat/alpine-php-wordpress/amd64.svg?style=for-the-badge)
![MicroBadger Size (tag)](https://img.shields.io/microbadger/image-size/canarysat/alpine-php-wordpress/amd64.svg?style=for-the-badge)

![Version](https://img.shields.io/badge/version-aarch64-blue.svg?style=for-the-badge)
![MicroBadger Layers (tag)](https://img.shields.io/microbadger/layers/canarysat/alpine-php-wordpress/aarch64.svg?style=for-the-badge)
![MicroBadger Size (tag)](https://img.shields.io/microbadger/image-size/canarysat/alpine-php-wordpress/aarch64.svg?style=for-the-badge)

![Version](https://img.shields.io/badge/version-armhf-blue.svg?style=for-the-badge)
![MicroBadger Layers (tag)](https://img.shields.io/microbadger/layers/canarysat/alpine-php-wordpress/armhf.svg?style=for-the-badge)
![MicroBadger Size (tag)](https://img.shields.io/microbadger/image-size/canarysat/alpine-php-wordpress/armhf.svg?style=for-the-badge)


## Volume structure

* `/usr/html`: Webroot


## Creating an instance

Make sure you create the folder on the host before starting the container and obtain the correct permissions.

```
mkdir -p /data/{domain}/html

docker run -e VIRTUAL_HOST={domain}.com,www.{domain}.com -v /data/{domain}/html:/usr/html -p 80:80 canarysat/alpine-php-wordpress:latest

E.G

mkdir -p /data/canarysat/html

docker run -e VIRTUAL_HOST=canarysat.com,www.canarysat.com -v /data/canarysat/html:/usr/html -p 80:80 canarysat/alpine-php-wordpress:latest
```

The following user and group id are used, the files should be set to this:
User ID:
Group ID:

```
chown -R 100:101 /data/{domain}/html

E.G

chown -R 100:101 /data/canarysat/html
```

Populate /data/{domain}/html with your WP files.


The following user and group id are used, the files should be set to this:

User ID:

Group ID:


```
chown -R 100:101 /data/{domain}/html
```

### WP-CLI

This image now includes WP-CLI wpcli.org baked in... Its best to `su nginx` before executing anything or else you can potentially compromise your host.

```
docker exec -it <container_name> bash
su nginx
cd /usr/html
wp-cli core download --locale=en_GB
```

### Redis Cache

Edit the wp-config.php file and include the line;

```
define('WP_REDIS_HOST', 'redis');
```

The next thing is to install the plugin [Redis Object Cache](https://wordpress.org/plugins/redis-cache/)


### SSL behind a proxy

If using SSL and running behind a proxy like HAproxy then the following needs to be added to the wp-config.php file (to stop infinite redirect);

```
define('FORCE_SSL_ADMIN', true);
if (strpos($_SERVER['HTTP_X_FORWARDED_PROTO'], 'https') !== false)
   $_SERVER['HTTPS']='on';
```

### Upload limit

The upload limit is 128 Megabytes.

### Change php.ini value
modify files/php-fpm.conf

To modify php.ini variable, simply edit php-fpm.ini and add php_flag[variable] = value.

```
php_flag[display_errors] = on
```

### PHP Modules
#### List of available modules in Alpine Linux, not all these are installed.
##### In order to install a php module do, (leave out the version number i.e. -5.6.11-r0
```
docker exec <image_id> apk add <pkg_name>
docker restart <image_name>
```
Example:

```
docker exec <image_id> apk add php7-soap
docker restart <image_name>
```

```
php7-common
php7-pdo_sqlite
php7-pear
php7-ftp
php7-imap
php7-mysqli
php7-json
php7-mbstring
php7-soap
php7-litespeed
php7-sockets
php7-bcmath
php7-opcache
php7-dom
php7-zlib
php7-gettext
php7-fpm
php7-intl
php7-openssl
php7-session
php7-mcrypt
php7-pdo_mysql
php7-embed
php7-xmlrpc
php7-wddx
php7-dba
php7-ldap
php7-xsl
php7-exif
php7-pdo_dblib
php7-bz2
php7-pdo
php7-pspell
php7-sysvmsg
php7-gmp
php7-apache2
php7-pdo_odbc
php7-shmop
php7-ctype
php7-phpdbg
php7-enchant
php7-sysvsem
php7-sqlite3
php7-odbc
php7-pcntl
php7-calendar
php7-xmlreader
php7-snmp
php7-zip
php7-posix
php7-iconv
php7-curl
php7-doc
php7-gd
php7-xml
php7-dev
php7-cgi
php7-sysvshm
php7-pgsql
php7-tidy
php7-pdo_pgsql
php7-phar
php7-mysqlnd
```

## Docker Compose example:

```yalm
wordpress:
  image: canarysat/alpine-php-wordpress:latest
  environment:
    VIRTUAL_HOST: example.com
  expose:
    - "80"
  volumes:
    - /data/example/www:/usr/html
  restart: always
  links:
    - mysql:mysql
mysql:
  environment:
    MYSQL_DATABASE: wordpressdb
    MYSQL_PASSWORD: wordpresspass
    MYSQL_ROOT_PASSWORD: 'YOUR-ROOT-MYSQL-PASSWORD'
    MYSQL_USER: wordpressuser
  image: canarysat/alpine-mariadb
```

## Source Repository

* [Github - canarysat/alpine-php-wordpress](https://github.com/canarysat/alpine-php-wordpress)

## Links

* [CanarySAT](https://www.canarysat.com/)

* [Dockerhub - canarysat](https://hub.docker.com/u/canarysat/)

## Donation

```
BITCOIN:
ETHEREUM:
ZCASH: 
```
