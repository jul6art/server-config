Configuration de serveurs
==
Migration complète d'un serveur
-
### [&#9756; Retour au menu](../README.md)
> !!! IMPORTANT !!! 

> TOUTES LES OPERATIONS DOIVENT ETRE EFFECTUEES SUR LE SERVEUR DE DESTINATION

Vérifier que l'OS est en place

    uname -a
    
Vérifier l'accès au serveur source

    ssh root@IP_DU_SERVEUR_SOURCE
    
Vérifier que rsync est installé

    which rsync
    
Liste des dossiers à exclaire du rsync

    /etc/fstab
    /etc/sysconfig/network-scripts/* 
    /etc/network/* 
    /proc/*
    /root/*
    /tmp/*
    /sys/*
    /dev/*
    /mnt/*
    /boot/*
    
    /etc/netplan/* 
    /etc/systemd/network/* 
    /etc/udev
    /etc/lvm
    /lib/modues
    
Commande à lancer

    rsync -auHxv –numeric-ids --exclude '/etc/fstab' --exclude '/etc/network/*' --exclude '/proc/*' --exclude '/tmp/*' --exclude '/sys/*' --exclude '/dev/*' --exclude '/mnt/*' --exclude '/boot/*' --exclude '/root/*' --exclude '/etc/netplan/*' --exclude '/etc/systemd/network/*' --exclude '/etc/udev' --exclude '/etc/lvm' --exclude '/lib/modues' root@51.15.140.136:/* /

> En cas d'erreur "invalid remote arg", il suffit de monter sur le serveur source et d'in verser le sens de la copie

Une fois la syncronisation terminée, redémarrer la machine

    reboot

&copy; 2019 [VsWeb](https://vsweb.be)