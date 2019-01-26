Configuration de serveurs
==
Certificat SSL
-
### [&#9756; Retour au menu](../README.md)
#### Certificat payant Comodo ou autre
Générer les fichiers key et csr et copier le contenu du fichier CSR généré sur https://ssls.com
> nom de domaine = COMMON NAME (sans www)
		
    mkdir /home/ssl && openssl req -new -newkey rsa:2048 -nodes -keyout /home/ssl/iroom.key -out /home/ssl/iroom.csr
    mkdir /home/ssl && openssl req -new -newkey rsa:2048 -nodes -keyout /home/ssl/lauto-resultat.key -out /home/ssl/lauto-resultat.csr

Si Virtualmin est installé, aller dans Server Configuration > Manage SSL > New Certificate

    put certificate text and PRIVATE KEY TEXT 
    active the certificate for usermin and webmin

#### Certificat gratuit
Si Virtualmin est installé, aller dans Server Configuration > Manage SSL > Let's ENCRYPT

    add all subdomains (without and with WWW) but NOT *.domaine.ext
		
#### Modifier ensuite le fichier de conf

    nano /etc/sites-enabled/site.conf
    
> modifier le virtualHost :443, changer IP.DU.SERVEUR:443 par *:443

> supprimer le RedirectMatch qui est dans le virtual host :443
 pour éviter une boucle infinie

#### Redémarrer apache

    service apache2 restart

&copy; 2019 [VsWeb](https://vsweb.be)