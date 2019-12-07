<p align="center">
    <a href="https://vsweb.be"><img src="https://vsweb.be/userfiles/images/14548837631453228685logo.png" alt="logo VsWeb"></a>
</p>

<p align="center">
    <a href="https://opensource.org/licenses/MIT" target="_blank"><img src="https://img.shields.io/badge/License-MIT-yellow.svg" alt="License"></a>
    <a href="https://github.com/jul6art/symfony-skeleton" target="_blank"><img src="https://img.shields.io/static/v1?label=stable&message=v1&color=success" alt="Version"></a>
</p>

Configuration de serveurs
=========================
Première connexion
------------------

Changer le mot de passe de l'utilisateur root
    
```console
passwd root   
```
    
Récupérer la clef ssh de notre machine

```console
cat ~/.ssh/id_rsa.pub
```
    
Ajouter la clef au serveur

```console
mkdir ~/.ssh && 
nano ~/.ssh/authorized_keys
```
    
Désactiver le timeout ssh

```console
nano /etc/ssh/sshd_config
```
    
Et ajouter les paramètres suivants

```console
TCPKeepAlive yes
ClientAliveInterval 60
ClientAliveCountMax 720
```
    
Redémarrer le service ssh

```console
service ssh restart
```
   

License
-------

The VsWeb Server Config is open-sourced software licensed under the MIT license.

&copy; 2019 [VsWeb](https://vsweb.be) 

















