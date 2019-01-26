Configuration de serveurs
==
Installation Apache sans Virtualmin
-
### [&#9756; Retour au menu](../README.md)
![logo Apache](https://doc.ubuntu-fr.org/_media/apache_logo.png "logo apache")

Installation

    sudo apt-get install apache2 -y &&
    service apache2 restart &&
    a2enmod rewrite

SSL

    a2enmod ssl &&
    service apache2 restart &&
    apt-get install python-certbot-apache -y
    
#### mariadb

    sudo apt-get install mariadb-server &&
    service apache2 restart

to start mariadb

    mariadb
        use mysql;
        update user set authentication_string=PASSWORD("root") where User='root';
        flush privileges;
        quit

#### postgres

    sudo apt-get install postgresql-server-dev-10 &&
    sudo apt-get install postgresql-10

to start postgres

    su postgres
    psql
    
### [&#9758; Enregistrements DNS](DNS.md)
### [&#9758; Certificat SSL](SSL.md)

&copy; 2019 [VsWeb](https://vsweb.be)