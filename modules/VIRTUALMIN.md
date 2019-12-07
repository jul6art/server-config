<p align="center">
    <a href="https://vsweb.be"><img src="https://vsweb.be/userfiles/images/14548837631453228685logo.png" alt="logo VsWeb"></a>
</p>

<p align="center">
    <a href="https://opensource.org/licenses/MIT" target="_blank"><img src="https://img.shields.io/badge/License-MIT-yellow.svg" alt="License"></a>
    <a href="https://github.com/jul6art/symfony-skeleton" target="_blank"><img src="https://img.shields.io/static/v1?label=stable&message=v1&color=success" alt="Version"></a>
</p>

Configuration de serveurs
=========================
Installation Virtualmin
-----------------------

![logo Virtualmin](https://www.virtualmin.com/images/virtualmin-logo-220x45.png "logo virtualmin")

Installation
------------

Téléchargement

```console
wget http://software.virtualmin.com/gpl/scripts/install.sh
```

Installation 
> dites oui à la question posée lors de l'installation et exemple de hostname défini si demandé: vsweb01.vsweb.be

```console
source install.sh 
```
    
Configuration
-------------

Dans webmin > webmin configuration > ports and ip address

    changer les deux 10000 by 8000
    
Activer DKIM

    dans email settings > domainKeys identified mail
    
> now you can install packages updates and re-check the config, correcting the errors like this

This virtual server is using the mod_php or FPM execution mode for PHP, such does not allow per-directory version selection.

    Dans virtualmin -> Server configuration -> Website options -> choisir fcgid

The mailman queue processor /usr/lib/mailman/bin/qrunner is not running on your system.

    System Settings -> Features and Plugins, et vous pouvez désactiver "Mailman feature"
    et "ENABLE SSL WEBSITE"
 
Mots-clefs: map, dependant
Sender Dependent Outgoing IP Address

```console
nano /etc/postfix/main.cf
```

> Chercher la ligne "sender_dependent_default_transport_maps" . Ajouter cette ligne si elle n'existe pas encore

```console
sender_dependent_default_transport_maps = hash:/etc/postfix/dependent
```
    
Permettre les emails smtp depuis Django

```console
smtpd_sender_restrictions = permit_mynetworks, warn_if_reject
```
    
Adapter pour respecter ceci
    
```console
myhostname = ##REVERSE_DNS_IPV4##
myorigin = ##REVERSE_DNS_IPV4##
mydestination = localhost.localdomain, localhost, ##REVERSE_DNS_IPV4##
relayhost =
mynetworks = 127.0.0.0/8, ##IPV4_PUBLIQUE_SERVEUR## [::1]/18
mailbox_size_limit = 0
recipient_delimiter = +
inet_interfaces = all
inet_protocols = ipv4
```

Commandes à lancer

```console
touch /etc/postfix/dependent && postmap hash:/etc/postfix/dependent &&
a2dissite 000-default.conf &&
service postfix restart && service apache2 restart
```
    
> Change les ips en cas de problème


Synchroniser les horloges
-------------------------

verifier l heure systeme
puis webmin/hardware/system time/update timezone (europe/brussels)
puis webmin/others/php configuration/other settings et changer la timezone pour les 3 lignes du tableau (manage)

> :warning: VERIFIER L HEURE EXACTE APRES LA CREATION DU PREMIER SERVEUR VIRTUEL (pour corriger un serveur: virtualmin/choix du serveur/services/phpX configuration et changer timezone)
    
Restauration en ligne de commandes

```console
virtualmin restore-domain --source /path/FILE.tar.gz --all-domains --all-features
```
    
Corriger problèmes d'accès DB
 
> Aller dans webmin > servers > mysql > databases privilèges et sélectionner la liste des droits


Modèles de fichiers de [conf Apache](VHOST.md)
[Enregistrements DNS](DNS.md)
Ajouter un [serveur virtuel](NEW_VIRTUAL_SERVER.md)
[Certificat SSL](SSL.md)


License
-------

The VsWeb Server Config is open-sourced software licensed under the [MIT license](https://opensource.org/licenses/MIT).

&copy; 2019 [VsWeb](https://vsweb.be)
