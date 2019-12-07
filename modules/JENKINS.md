<p align="center">
    <a href="https://vsweb.be"><img src="https://vsweb.be/userfiles/images/14548837631453228685logo.png" alt="logo VsWeb"></a>
</p>

<p align="center">
    <a href="https://opensource.org/licenses/MIT" target="_blank"><img src="https://img.shields.io/badge/License-MIT-yellow.svg" alt="License"></a>
    <a href="https://github.com/jul6art/symfony-skeleton" target="_blank"><img src="https://img.shields.io/static/v1?label=stable&message=v1&color=success" alt="Version"></a>
</p>

Configuration de serveurs
=========================
Intégration continue
--------------------

![logo Jenkins](https://wiki.jenkins.io/download/attachments/2916393/logo.png?version=1&modificationDate=1302753947000&api=v2 "logo jenkins")

Installation
------------

Installation de ant
```console
sudo apt-get install ant
```
    
Installation de Java

```console
sudo apt-get install openjdk-8-jdk
```
    
Installation de Jenkins

```console
wget -q -O - https://pkg.jenkins.io/debian/jenkins.io.key | sudo apt-key add - &&
sudo sh -c 'echo deb http://pkg.jenkins.io/debian-stable binary/ > /etc/apt/sources.list.d/jenkins.list' &&
sudo apt-get update &&
sudo apt-get install jenkins &&
systemctl status jenkins &&
sudo ufw allow 8080
```
    
Tester le statut

```console
sudo ufw status
```

#### Configuration
aller sur le port 8080 de l'IP du serveur dans le navigateur

    http://IP.DU.SERVEUR:8080
    
installer les plugins suivants

    php, ansicolor, slack notification, bitbucket plugin
    
Créer de nouveaux credentials avec votre login/mot de passe github ou bitbucket
    
Avant d'intégrer un nouveau projet, il est préférable de remettre le repository Git à zéro pour éviter les dossiers .git trop volumineux

Pour intégrer un nouveau projet, il suffit de créer un hook sur github dans le projet

    https://IP.DU.SERVEUR:8080/bitbucket-hook/
    
Et créer un fichier build.xml dans le projet contenant des tâches ANT à appeler dans la configuration du projet

Paramètres (créer un nouveau projet vide)

* définir la source git
* définir comme projet bitbucket avec la source et la branche
* définir les tâches ant: build et deploy (+ paramètres)

Paramètres pour intégrer slack

* url de base: https://IDENTIFIANT.slack.com/services/hooks/jenkins-ci/
* jeton d'intégration: HiSrRvY3nZa7KNvvtgOA6rxg
* page de doc: https://IDENTIFIANT.slack.com/apps/manage
* message custom: *NOM DU PROJECT - BRANCH*
    
Redirection vers un nom de domaine
----------------------------------
Activer les modules apaches

    sudo a2enmod headers proxy proxy_http ssl proxy_wstunnel rewrite
    
Redémarrer Apache

```console
service apache2 restart
```
    
Fichier de configuration du serveur virtuel

```console
<VirtualHost *:80>
    ServerAdmin admin@vsweb.be

    ServerName jenkins.vsweb.be
    ServerAlias www.jenkins.vsweb.be

    ProxyPreserveHost On
    
    ProxyPass / http://127.0.0.1:8080/
    ProxyPassReverse / http://127.0.0.1:8080/

    ErrorLog ${APACHE_LOG_DIR}/jenkins_error.log
    CustomLog ${APACHE_LOG_DIR}/jenkins_access.log combined
</VirtualHost>
```
   

License
-------

The VsWeb Server Config is open-sourced software licensed under the MIT license.

&copy; 2019 [VsWeb](https://vsweb.be)
