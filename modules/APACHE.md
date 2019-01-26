Configuration de serveurs
==
Installation Apache sans Virtualmin
-
### [&#9756; Retour au menu](../README.md)
Installation

    sudo apt-get install apache2 -y &&
    service apache2 restart &&
    a2enmod rewrite

SSL

    a2enmod ssl &&
    service apache2 restart &&
    apt-get install python-certbot-apache -y

&copy; 2019 [VsWeb](https://vsweb.be)