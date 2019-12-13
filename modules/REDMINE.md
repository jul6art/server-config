<p align="center">
    <a href="https://devinthehood.com"><img src="https://github.com/jul6art/slim-skeleton/blob/master/assets/img/logo.png?raw=true" alt="logo dev in the hood"></a>
</p>

<p align="center">
    <a href="https://opensource.org/licenses/MIT" target="_blank"><img src="https://img.shields.io/badge/License-MIT-yellow.svg" alt="License"></a>
    <a href="https://github.com/jul6art/server-config" target="_blank"><img src="https://img.shields.io/static/v1?label=stable&message=v1&color=success" alt="Version"></a>
</p>

Configuration de serveurs
=========================
Site de support Redmine
-----------------------

Créer un serveur virtuel
------------------------

> This part is pretty much self-explanatory: create a new subserver named ìredmine.example.com under the server ìexample.com from the Virtualmin dashboard.

Installation

```console
sudo apt-get install redmine redmine-mysql libapache2-mod-passenger
```


Activer les modules Apache

```console
a2enmod passenger rewrite
```

Fichier de configuration

```apacheconfig
<VirtualHost your-ip-here:80>
  SuexecUserGroup "#1010" "#1010"
  ServerName redmine.example.com
  DocumentRoot "/usr/share/redmine/public"
  PassengerResolveSymlinksInDocumentRoot on
  Options Indexes ExecCGI FollowSymlinks
  ErrorLog /var/log/virtualmin/redmine.example.com_error_log
  CustomLog /var/log/virtualmin/redmine.example.com_access_log combined
</VirtualHost>
```

Puis

```console
nano /etc/redmine/default/configuration.yml
```
    
Et ajouter

```console
production:
 email_delivery:
  delivery_method: sendmail
```
 
Puis 

```console
nano /etc/redmine/default/database.yml
```
    
Et ajouter database support user "support" in configuration of database

  
Redémarrer Apache

```console
service apache2 restart
```

> Navigate to http://redmine.example.com and log in with the default admin / admin credentials. Change these via the My Account link, and then go to Administration > Settings and change the hostname from localhost:3000 to redmine.example.com

> :warning: Attention uploads images et git (plugin redmine git remote et droits de fichiers et dossiers et enfants de dossiers)

Corrections

```console
chown -rf support:www-data /usr/share/redmine
chown -rf support:www-data /usr/share/redmine/Gemfile.lock
chown -rf support:www-data /etc/redmine
chmod -R 755 /var/lib/redmine/public/plugin_assets
chmod -R 755 /var/lib/redmine/files
```
    
> Dans administration / configuration, vérifier l'onglet "informations"
    
> En cas d'erreur sur certaines pages, chmod -R 777 /usr/share/redmine/tmp

Plugins
 
```console
cd /usr/share/redmine/plugins &&
git clone https://github.com/dergachev/redmine_git_remote &&
service apache2 restart
```

Test

Charge datas or test img, emails and git repos (beware of disk quota for user support)


License
-------

The Server Config is open-sourced software licensed under the [MIT license](https://opensource.org/licenses/MIT).

&copy; 2019 [dev in the hood](https://devinthehood.com) 

















