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

#### postgres 11

    wget --quiet -O - https://www.postgresql.org/media/keys/ACCC4CF8.asc | sudo apt-key add -
    RELEASE=$(lsb_release -cs)
    echo "deb http://apt.postgresql.org/pub/repos/apt/ ${RELEASE}"-pgdg main | sudo tee  /etc/apt/sources.list.d/pgdg.list
    
> Vérifier que le fichier "cat /etc/apt/sources.list.d/pgdg.list" contient bien "deb http://apt.postgresql.org/pub/repos/apt/ stretch-pgdg main"

    sudo apt update
    sudo apt -y install postgresql-11

to start postgres

    su postgres
    psql
    
### [&#9758; Enregistrements DNS](DNS.md)
### [&#9758; Certificat SSL](SSL.md)

&copy; 2019 [VsWeb](https://vsweb.be)
