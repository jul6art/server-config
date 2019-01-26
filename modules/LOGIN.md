Configuration de serveurs
==
Première connexion
-
### [&#9756; Retour au menu](../README.md)
Changer le mot de passe de l'utilisateur root
    
    passwd root   
    
Récupérer la clef ssh de notre machine

    cat ~/.ssh/id_rsa.pub
    
Ajouter la clef au serveur

    mkdir ~/.ssh && 
    sudo nano ~/.ssh/authorized_keys
    
Désactiver le timeout ssh

    nano /etc/ssh/sshd_config
    
Et ajouter les paramètres suivants

    TCPKeepAlive yes
    ClientAliveInterval 60
    ClientAliveCountMax 720
    
Redémarrer le service ssh

    service ssh restart
   
&copy; 2019 [VsWeb](https://vsweb.be) 

















