Configuration de serveurs
==
Plusieurs PHP en parallèle
-
### [&#9756; Retour au menu](../README.md)
![logo PHP](http://php.net//images/logos/new-php-logo.svg "logo php")

#### Installation

	sudo apt-get -y install apt-transport-https lsb-release ca-certificates &&
	sudo curl -ssL -o /etc/apt/trusted.gpg.d/php.gpg https://packages.sury.org/php/	apt.gpg &&
	sudo sh -c 'echo "deb https://packages.sury.org/php/ $(lsb_release -sc) main" 	> /etc/apt/sources.list.d/php.list' &&
	sudo apt-get update &&
	sudo apt-get install php5.6-cli php7.0-cli php7.1-cli php7.2-cli php7.3-cli &&
	sudo apt-get install php5.6-fpm php7.0-fpm php7.1-fpm php7.2-fpm php7.3-fpm
	
Modules PHP

    sudo apt-get install php5.6-gd php5.6-mcrypt php5.6-curl php5.6-opcache php5.6-mysql php5.6-pgsql &&
    sudo apt-get install php7.0-gd php7.0-mcrypt php7.0-curl php7.0-opcache php7.0-mysql php7.0-pgsql &&
    sudo apt-get install php7.1-gd php7.1-mcrypt php7.1-curl php7.1-opcache php7.1-mysql php7.1-pgsql &&
    sudo apt-get install php7.2-gd php7.2-curl php7.2-opcache php7.2-mysql php7.2-pgsql &&
    sudo apt-get install php7.3-gd php7.3-curl php7.3-opcache php7.3-mysql php7.3-pgsql
    
Manipulation d'images

    sudo apt-get install jpegoptim optipng pngquant gifsicle &&
    sudo apt-get install npm && sudo npm install -g svgo

Composer

    php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');"
    php -r "if (hash_file('sha384', 'composer-setup.php') === '93b54496392c062774670ac18b134c3b3a95e5a5e5c8f1a9f115f203b75bf9a129d5daa8ba6a13e2cc8a1da0806388a8') { echo 'Installer verified'; } else { echo 'Installer corrupt'; unlink('composer-setup.php'); } echo PHP_EOL;"
    php composer-setup.php
    php -r "unlink('composer-setup.php');"
    mv composer.phar /usr/local/bin/composers
	
Module apache proxy_fcgi

    a2enmod proxy_fcgi &&
    service apache2 restart
    
> Si virtualmin est installé, la dernière étape est d'installer les modules phpX.X-cgi et redémarrer le fpm

> sudo apt-get install php5.6-cgi php7.0-cgi php7.1-cgi php7.2-cgi php7.3-cgi
    
#### Configuration (sauf si virtualmin est installé)
> Refaire pour chaque version de PHP avec des ports différents
    
    nano /etc/php/5.6/fpm/pool.d/www.conf

changer

    listen = /run/php/php5.6-fpm.sock

par
	
	listen = 127.0.0.1:9056
	
#### Utilisation dans un fichier de conf apache (sauf si virtualmin est installé)

    <FilesMatch "\.php$">
            Require all granted
            SetHandler proxy:fcgi://127.0.0.1:9056
    </FilesMatch>

&copy; 2019 [VsWeb](https://vsweb.be)
