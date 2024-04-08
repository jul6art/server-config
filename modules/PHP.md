<p align="center">
    <a href="https://devinthehood.com"><img src="https://github.com/jul6art/slim-skeleton/blob/master/assets/img/logo.png?raw=true" alt="logo dev in the hood"></a>
</p>

<p align="center">
    <a href="https://opensource.org/licenses/MIT" target="_blank"><img src="https://img.shields.io/badge/License-MIT-yellow.svg" alt="License"></a>
    <a href="https://github.com/jul6art/server-config" target="_blank"><img src="https://img.shields.io/static/v1?label=stable&message=v1&color=success" alt="Version"></a>
</p>

Configuration de serveurs
=========================
Plusieurs PHP en parallèle
--------------------------

![logo PHP](http://php.net//images/logos/new-php-logo.svg "logo php")

Installation
------------

First install CURL

```shell
sudo apt-get -y install curl
```
Then you can install PHP

```shell
sudo apt-get -y install apt-transport-https lsb-release ca-certificates &&
sudo curl -ssL -o /etc/apt/trusted.gpg.d/php.gpg https://packages.sury.org/php/	apt.gpg &&
sudo sh -c 'echo "deb https://packages.sury.org/php/ $(lsb_release -sc) main" 	> /etc/apt/sources.list.d/php.list' &&
sudo apt-get update
```
	
PHP Versions

```shell
sudo apt-get install php7.4-cli php7.4-fpm php7.4-intl php7.4-soap php7.4-gd php7.4-mbstring php7.4-curl php7.4-zip php7.4-opcache php7.4-mysql php7.4-pgsql php7.4-xml php7.4-apc -y &&
sudo apt-get install php8.0-cli php8.0-fpm php8.0-intl php8.0-soap php8.0-gd php8.0-mbstring php8.0-curl php8.0-zip php8.0-opcache php8.0-mysql php8.0-pgsql php8.0-xml php8.0-apc -y &&
sudo apt-get install php8.1-cli php8.1-fpm php8.1-intl php8.1-soap php8.1-gd php8.1-mbstring php8.1-curl php8.1-zip php8.1-opcache php8.1-mysql php8.1-pgsql php8.1-xml php8.1-apc -y &&
sudo apt-get install php8.2-cli php8.2-fpm php8.2-intl php8.2-soap php8.2-gd php8.2-mbstring php8.2-curl php8.2-zip php8.2-opcache php8.2-mysql php8.2-pgsql php8.2-xml php8.2-apc -y &&
sudo apt-get install php8.3-cli php8.3-fpm php8.3-intl php8.3-soap php8.3-gd php8.3-mbstring php8.3-curl php8.3-zip php8.3-opcache php8.3-mysql php8.3-pgsql php8.3-xml php8.3-apc -y
```
	
Configuration (php.ini)

```shell
expose_php = Off     
```

> To not display PHP version in headers
    
Manipulation d'images

```shell
sudo apt-get install jpegoptim optipng pngquant gifsicle -y

curl -sL https://deb.nodesource.com/setup_16.x | bash -

sudo apt-get install -y nodejs npm && sudo npm install -g svgo
```

Composer

```shell
php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');"

php -r "if (hash_file('sha384', 'composer-setup.php') === '55ce33d7678c5a611085589f1f3ddf8b3c52d662cd01d4ba75c0ee0459970c2200a51f492d557530c71c15d8dba01eae') { echo 'Installer verified'; } else { echo 'Installer corrupt'; unlink('composer-setup.php'); } echo PHP_EOL;"

php composer-setup.php

php -r "unlink('composer-setup.php');"

mv composer.phar /usr/local/bin/composer
```

Module apache proxy_fcgi

```shell
a2enmod proxy_fcgi &&
service apache2 restart
```
    
> Si virtualmin est installé, les dernières étapes sont d'installer les modules phpX.X-cgi et redémarrer le fpm

>   sudo apt-get install php7.4-cgi php8.0-cgi php8.1-cgi php8.2-cgi php8.3-cgi -y
    
Configuration (sauf si virtualmin est installé)
-----------------------------------------------

> :warning: Répéter les opérations pour chaque version de PHP avec des ports différents
    
    nano /etc/php/5.6/fpm/pool.d/www.conf

changer

```shell
listen = /run/php/php5.6-fpm.sock
```

par
	
```shell
listen = 127.0.0.1:9056
```
	
Utilisation dans un fichier de conf apache (sauf si virtualmin est installé)
----------------------------------------------------------------------------

```shell
<FilesMatch "\.php$">
        Require all granted
        SetHandler proxy:fcgi://127.0.0.1:9056
</FilesMatch>
```


License
-------

The Server Config is open-sourced software licensed under the [MIT license](https://opensource.org/licenses/MIT).

&copy; 2019 [dev in the hood](https://devinthehood.com)
