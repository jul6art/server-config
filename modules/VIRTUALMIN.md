Configuration de serveurs
==
Installation Virtualmin
-
### [&#9756; Retour au menu](../README.md)
![logo Virtualmin](https://www.virtualmin.com/images/virtualmin-logo-220x45.png "logo virtualmin")

#### Installation

Téléchargement

    wget http://software.virtualmin.com/gpl/scripts/install.sh

Installation 
> dites oui à la question posée lors de l'installation et exemple de hostname défini si demandé: vsweb01.vsweb.be

    source install.sh 
    
#### Configuration
> now you can install packages updates and re-check the config, correcting the errors like this

The mailman queue processor /usr/lib/mailman/bin/qrunner is not running on your system.

    System Settings -> Features and Plugins, et vous pouvez désactiver "Mailman feature"
    et "ENABLE SSL WEBSITE"

Dans webmin > webmin configuration > ports and ip address

    changer les deux 10000 by 8000
 
Mots-clefs: map, dependant
Sender Dependent Outgoing IP Address

    nano /etc/postfix/main.cf

> Chercher la ligne "sender_dependent_default_transport_maps" . Ajouter cette ligne si elle n'existe pas encore

    sender_dependent_default_transport_maps = hash:/etc/postfix/dependent
    
Permettre les emails smtp depuis Django

    smtpd_sender_restrictions = permit_mynetworks, warn_if_reject
    
Adapter pour respecter ceci
    
    myhostname = ##REVERSE_DNS_IPV4##
    myorigin = ##REVERSE_DNS_IPV4##
    mydestination = localhost.localdomain, localhost, ##REVERSE_DNS_IPV4##
    relayhost =
    mynetworks = 127.0.0.0/8, ##IPV4_PUBLIQUE_SERVEUR## [::1]/18
    mailbox_size_limit = 0
    recipient_delimiter = +
    inet_interfaces = all
    inet_protocols = ipv4

Commandes à lancer

    touch /etc/postfix/dependent && postmap hash:/etc/postfix/dependent &&
    a2dissite 000-default.conf &&
    service postfix restart && service apache2 restart
    
> Change les ips en cas de problème


Synchroniser les horloges

    verifier l heure systeme
    puis webmin/hardware/system time/update timezone (europe/brussels)
    puis webmin/others/php configuration/other settings et changer la timezone pour les 3 lignes du tableau (manage)
    VERIFIER L HEURE EXACTE APRES LA CREATION DU PREMIER SERVEUR VIRTUEL (pour corriger un serveur: virtualmin/choix du serveur/services/phpX configuration et changer timezone)
    
Restauration en ligne de commandes

    virtualmin restore-domain --source /path/FILE.tar.gz --all-domains --all-features



### [&#9758; Modèles de fichiers de conf Apache](VHOST.md)
### [&#9758; Enregistrements DNS](DNS.md)
### [&#9758; Ajouter un serveur virtuel](NEW_VIRTUAL_SERVER.md)
### [&#9758; Certificat SSL](SSL.md)

&copy; 2019 [VsWeb](https://vsweb.be)
