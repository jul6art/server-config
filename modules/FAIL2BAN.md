<p align="center">
    <a href="https://vsweb.be"><img src="https://vsweb.be/userfiles/images/14548837631453228685logo.png" alt="logo VsWeb"></a>
</p>

<p align="center">
    <a href="https://opensource.org/licenses/MIT" target="_blank"><img src="https://img.shields.io/badge/License-MIT-yellow.svg" alt="License"></a>
    <a href="https://github.com/jul6art/symfony-skeleton" target="_blank"><img src="https://img.shields.io/static/v1?label=stable&message=v1&color=success" alt="Version"></a>
</p>

Configuration de serveurs
=========================
Fail 2 ban
----------

![logo Fail2ban](https://doc.ubuntu-fr.org/_media/fail2ban_logo.png "logo fail2ban")

Installation
------------

```console
sudo apt-get install fail2ban
```
    
Configuration
-------------

```console
cp /etc/fail2ban/jail.conf /etc/fail2ban/jail.local && 
nano /etc/fail2ban/jail.local
```
    
Modifier comme ceci

```console
#### [DEFAULT]

    ignoreip = 127.0.0.1/8 ##YOUR_HOME_IP##
    bantime = 3600
    findtime = 3600   
    maxretry = 6      
    mta = mail                     
    destemail = admin@vsweb.be
    sendername = Fail2BanAlerts
    action = %(action_mwl)s
    
#### [apache]

    enabled  = true
    port     = http,https
    filter   = apache-auth
    logpath  = /var/log/apache*/*error.log
    maxretry = 6

#### [apache-noscript]

    enabled  = true

#### [apache-overflows]

    enabled  = true
    port     = http,https
    filter   = apache-overflows
    logpath  = /var/log/apache*/*error.log
    maxretry = 2

#### [apache-badbots]

    enabled  = true
    port     = http,https
    filter   = apache-badbots
    logpath  = /var/log/apache*/*error.log
    maxretry = 2

#### [php-url-fopen]

    enabled = true
    port    = http,https
    filter  = php-url-fopen
    logpath = /var/log/apache*/*access.log
    
#### activer aussi proftpd, postfix, ...
```

Redémarrer
----------

```console
sudo service fail2ban restart
```
    
> :warning: En cas d'erreur "Failed to start fail2ban.service: Unit fail2ban.service is masked", lancer la commande "systemctl unmask fail2ban" avant de redémarrer
   

License
-------

The VsWeb Server Config is open-sourced software licensed under the MIT license.

&copy; 2019 [VsWeb](https://vsweb.be) 
