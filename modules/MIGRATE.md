<p align="center">
    <a href="https://devinthehood.com"><img src="https://github.com/jul6art/slim-skeleton/blob/master/assets/img/logo.png?raw=true" alt="logo dev in the hood"></a>
</p>

<p align="center">
    <a href="https://opensource.org/licenses/MIT" target="_blank"><img src="https://img.shields.io/badge/License-MIT-yellow.svg" alt="License"></a>
    <a href="https://github.com/jul6art/server-config" target="_blank"><img src="https://img.shields.io/static/v1?label=stable&message=v1&color=success" alt="Version"></a>
</p>

Configuration de serveurs
=========================
Migration complète d'un serveur
-------------------------------

> :warning: TOUTES LES OPERATIONS DOIVENT ETRE EFFECTUEES SUR LE SERVEUR DE DESTINATION

Vérifier que l'OS est en place

```shell
uname -a
```
    
Vérifier l'accès au serveur source


```shell
ssh root@IP_DU_SERVEUR_SOURCE
```
    
Vérifier que rsync est installé


```shell
which rsync
```
    
Liste des dossiers à exclaire du rsync

* /etc/fstab
* /etc/sysconfig/network-scripts/* 
* /etc/network/* 
* /proc/*
* /root/*
* /tmp/*
* /sys/*
* /dev/*
* /mnt/*
* /boot/*
    
* /etc/netplan/* 
* /etc/systemd/network/* 
* /etc/udev
* /etc/lvm
* /lib/modues
    
Commande à lancer

```shell
rsync -auHxv –numeric-ids --exclude '/etc/fstab' --exclude '/etc/network/*' --exclude '/proc/*' --exclude '/tmp/*' --exclude '/sys/*' --exclude '/dev/*' --exclude '/mnt/*' --exclude '/boot/*' --exclude '/root/*' --exclude '/etc/netplan/*' --exclude '/etc/systemd/network/*' --exclude '/etc/udev' --exclude '/etc/lvm' --exclude '/lib/modues' root@51.15.140.136:/* /
```

> En cas d'erreur "invalid remote arg", il suffit de monter sur le serveur source et d'in verser le sens de la copie

Redémarrer la machine
----------------------------------------------------------

```shell
reboot
```


License
-------

The Server Config is open-sourced software licensed under the [MIT license](https://opensource.org/licenses/MIT).

&copy; 2019 [dev in the hood](https://devinthehood.com)
