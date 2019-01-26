Configuration de serveurs
==
Installation Virtualmin
-
### [&#9756; Retour au menu](../README.md)
#### Installation

Téléchargement

    wget http://software.virtualmin.com/gpl/scripts/install.sh

Installation 
> dites oui à la question posée lors de l'installation et exemple de hostname défini si demandé: vsweb01.vsweb.be

    source install.sh 
    
#### Configuration

# now you can install packages updates and re-check the config, correcting the errors like this
#
# The mailman queue processor /usr/lib/mailman/bin/qrunner is not running on your system.
System Settings -> Features and Plugins, and in there you can disable the Mailman feature
and ENABLE SSL WEBSITE

# webmin
# webmin configuration
# ports and ip address
change the two 10000 by 8000
 
# mots-clefs: map, dependant
# Sender Dependent Outgoing IP Address
nano /etc/postfix/main.cf
# Look for a line starting with sender_dependent_default_transport_maps . If it already exists, your system is already ready.
# Otherwise, add the line 
sender_dependent_default_transport_maps = hash:/etc/postfix/dependent
# allow smtp django emails
smtpd_sender_restrictions = permit_mynetworks, warn_if_reject
# adapter pour respecter ceci
myhostname = ##REVERSE_DNS_IPV4##
myorigin = ##REVERSE_DNS_IPV4##
mydestination = localhost.localdomain, localhost, ##REVERSE_DNS_IPV4##
relayhost =
mynetworks = 127.0.0.0/8, ##IPV4_PUBLIQUE_SERVEUR## [::1]/18
mailbox_size_limit = 0
recipient_delimiter = +
inet_interfaces = all
inet_protocols = ipv4

#  Run the commands 
touch /etc/postfix/dependent && postmap hash:/etc/postfix/dependent &&
a2dissite 000-default.conf && apt-get install php5-gd php5-mcrypt php5-curl php-apc -y && 
service postfix restart && service apache2 restart
# change ips if problem


#IMPORTANT synchroniser les horloges
#IMPORTANT synchroniser les horloges
#IMPORTANT synchroniser les horloges
#IMPORTANT synchroniser les horloges
#IMPORTANT synchroniser les horloges
#IMPORTANT synchroniser les horloges
#IMPORTANT synchroniser les horloges
#IMPORTANT synchroniser les horloges
#IMPORTANT synchroniser les horloges
#IMPORTANT synchroniser les horloges
#IMPORTANT synchroniser les horloges
#IMPORTANT synchroniser les horloges
#IMPORTANT synchroniser les horloges
#IMPORTANT synchroniser les horloges
# verifier l heure systeme
# puis webmin/hardware/system time/update timezone (europe/brussels)
# puis webmin/others/php configuration/other settings et changer la timezone pour les 3 lignes du tableau (manage)
# VERIFIER L HEURE EXACTE APRES LA CREATION DU PREMIER SERVEUR VIRTUEL (pour corriger un serveur: virtualmin/choix du serveur/services/php5 configuration et changer timezone)



### [&#9758; Modèles de fichiers de conf Apache](VHOST.md)
### [&#9758; Enregistrements DNS](DNS.md)
### [&#9758; Ajouter un serveur virtuel](NEW_VIRTUAL_SERVER.md)
### [&#9758; Certificat SSL](SSL.md)

&copy; 2019 [VsWeb](https://vsweb.be)