<p align="center">
    <a href="https://vsweb.be"><img src="https://vsweb.be/userfiles/images/14548837631453228685logo.png" alt="logo VsWeb"></a>
</p>

<p align="center">
    <a href="https://opensource.org/licenses/MIT" target="_blank"><img src="https://img.shields.io/badge/License-MIT-yellow.svg" alt="License"></a>
    <a href="https://github.com/jul6art/symfony-skeleton" target="_blank"><img src="https://img.shields.io/static/v1?label=stable&message=v1&color=success" alt="Version"></a>
</p>

Configuration de serveurs
=========================
Installation Apache sans Virtualmin
-----------------------------------

![logo Apache](https://doc.ubuntu-fr.org/_media/apache_logo.png "logo apache")

Installation
------------
```console
sudo apt-get install apache2 -y &&
service apache2 restart &&
a2enmod rewrite
```

SSL
---

```console
a2enmod ssl &&
service apache2 restart &&
apt-get install python-certbot-apache -y
```
    
Mariadb
-------

```console
sudo apt-get install mariadb-server &&
service apache2 restart
```

to start mariadb

```console
mariadb
```

```sql
use mysql;
    update user set authentication_string=PASSWORD("root") where User='root';
    flush privileges;
    quit
```

Postgres 11
-----------

```console
wget --quiet -O - https://www.postgresql.org/media/keys/ACCC4CF8.asc | sudo apt-key add -

sudo sh -c 'echo "deb http://apt.postgresql.org/pub/repos/apt/ $(lsb_release -cs)-pgdg main"  > /etc/apt/sources.list.d/pgdg.list' && 
sudo apt update &&
sudo apt -y install postgresql-11
```

to start postgres
```console
su postgres
psql
```

License
-------

The VsWeb Server Config is open-sourced software licensed under the [MIT license](https://opensource.org/licenses/MIT).

&copy; 2019 [VsWeb](https://vsweb.be)
