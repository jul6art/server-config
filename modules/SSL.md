Configuration de serveurs
==
Certificat SSL
-
### [&#9756; Retour au menu](../README.md)
### Installation dans Virtualmin
#### Certificat payant Comodo ou autre
Générer les fichiers key et csr et copier le contenu du fichier CSR généré sur https://ssls.com
> nom de domaine = COMMON NAME (sans www)
		
    mkdir /home/ssl && openssl req -new -newkey rsa:2048 -nodes -keyout /home/ssl/vsweb.key -out /home/ssl/vsweb.csr
    mkdir /home/ssl && openssl req -new -newkey rsa:2048 -nodes -keyout /home/ssl/lauto-resultat.key -out /home/ssl/lauto-resultat.csr

Aller dans Server Configuration > Manage SSL > New Certificate

    put certificate text and PRIVATE KEY TEXT 
    active the certificate for usermin and webmin

#### Certificat gratuit
Aller dans Server Configuration > Manage SSL > Let's ENCRYPT

    add all subdomains (without and with WWW) but NOT *.domaine.ext
		
#### Modifier ensuite le fichier de conf

    nano /etc/sites-enabled/site.conf
    
> modifier le virtualHost :443, changer IP.DU.SERVEUR:443 par *:443

> supprimer le RedirectMatch qui est dans le virtual host :443
 pour éviter une boucle infinie
 
### Installation sans Virtualmin

    a2enmod ssl &&
    apt-get install certbot python-certbot-apache &&
    sudo certbot --apache
    
> Choisir ceux a activer

> Tester le renouvellement AUTOMATIQUE (fait par le CRON de virtualmin ?)

    sudo certbot renew --dry-run
    
> Documentation pour générer un certificat wildcard

    https://www.nikio.io/infrastructure/lets-encrypt-wildcard-certificate-with-certbot/
    
    wget https://dl.google.com/go/go1.11.5.linux-amd64.tar.gz
  cd ~/
  tar -C /usr/local -xzf go1.11.5.linux-amd64.tar.gz
  export PATH=$PATH:/usr/local/go/bin
  go version
  go get github.com/joohoi/acme-dns/...
  rm go1.11.5.linux-amd64.tar.gz
  sudo mv go/bin/acme-dns /usr/local/bin/acme-dns
  rm -rf go
  which acme-dns
  
 
Exemple de configuration (avec le module apache, elle est adaptée AUTOMATIQUEMENT)
#### Problèmes récurrents
> *:80 ou *:443 ne marche pas mais IP_DU_SERVEUR:80 oui

> En cas de proxy, pour Jenkins par exemple, la version 443 redirige vers https://127.0.0.1:8080 au lieu de http://127.0.0.1:8080

    <VirtualHost *:80 *:443>                     						
            ServerAdmin admin@vsweb.be
            ServerName www.vsweb.be
            ServerAlias vsweb.be               					
            DocumentRoot /home/vsweb/public_html/server
            ErrorLog /home/vsweb/logs/error.log
            CustomLog /home/vsweb/logs/access.log combined
            
            SSLEngine on									
            SSLOptions +StrictRequire							
            SSLCertificateChainFile /etc/letsencrypt/live/vsweb.be/chain.pem		
            SSLCertificateFile /etc/letsencrypt/live/vsweb.be/cert.pem		
            SSLCertificateKeyFile /etc/letsencrypt/live/vsweb.be/privkey.pem		
            RewriteEngine On								
            RewriteCond %{HTTPS} off							
            RewriteRule (.*) https://%{HTTP_HOST}%{REQUEST_URI}				
    </VirtualHost>


#### Redémarrer apache

    service apache2 restart

&copy; 2019 [VsWeb](https://vsweb.be)
