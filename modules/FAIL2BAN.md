Configuration de serveurs
==
Fail 2 ban
-
### [&#9756; Retour au menu](../README.md)
![logo Fail2ban](https://doc.ubuntu-fr.org/_media/fail2ban_logo.png "logo fail2ban")

Installation

    sudo apt-get install fail2ban
    
Configuration

    cp /etc/fail2ban/jail.conf /etc/fail2ban/jail.local && 
    nano /etc/fail2ban/jail.local
    
Modifier comme ceci

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

Redémarrer

    sudo service fail2ban restart
    
> En cas d'erreur "Failed to start fail2ban.service: Unit fail2ban.service is masked", lancer la commande "systemctl unmask fail2ban" avant de redémarrer
   
&copy; 2019 [VsWeb](https://vsweb.be) 
