<p align="center">
    <a href="https://devinthehood.com"><img src="https://github.com/jul6art/slim-skeleton/blob/master/assets/img/logo.png?raw=true" alt="logo dev in the hood"></a>
</p>

<p align="center">
    <a href="https://opensource.org/licenses/MIT" target="_blank"><img src="https://img.shields.io/badge/License-MIT-yellow.svg" alt="License"></a>
    <a href="https://github.com/jul6art/server-config" target="_blank"><img src="https://img.shields.io/static/v1?label=stable&message=v1&color=success" alt="Version"></a>
</p>

Configuration de serveurs
=========================
Certificat SSL
--------------

Installation dans Virtualmin
----------------------------

Certificat payant Comodo ou autre
---------------------------------

Générer les fichiers key et csr et copier le contenu du fichier CSR généré sur https://ssls.com

> nom de domaine = COMMON NAME (sans www)
	
```shell
mkdir /home/ssl && openssl req -new -newkey rsa:2048 -nodes -keyout /home/ssl/devinthehood.key -out /home/ssl/devinthehood.csr
mkdir /home/ssl && openssl req -new -newkey rsa:2048 -nodes -keyout /home/ssl/lauto-resultat.key -out /home/ssl/lauto-resultat.csr
```

Aller dans Server Configuration > Manage SSL > New Certificate

```shell
put certificate text and PRIVATE KEY TEXT 
active the certificate for usermin and webmin
```

Certificat gratuit
------------------

Aller dans Server Configuration > Manage SSL > Let's ENCRYPT

```shell
add all subdomains (without and with WWW) but NOT *.domaine.ext
```
		
Modifier ensuite le fichier de conf

```shell
nano /etc/sites-enabled/site.conf
```
    
> modifier le virtualHost :443, changer IP.DU.SERVEUR:443 par *:443

> supprimer le RedirectMatch qui est dans le virtual host :443
 pour éviter une boucle infinie
 
 :warning: modifier les **public_html_path** différents de **public_html** dans le fichier de config de chaque domain pour éviter les problèmes de renew

```shell
virtualmin list-domains --domain example.com --id-only
/etc/webmin/virtual-server/domains/ID_DU_DOMAIN
```
 
Installation sans Virtualmin
----------------------------

```shell
a2enmod ssl &&
apt-get install certbot python-certbot-apache &&
sudo certbot --apache
```
    
Choisir ceux a activer

Tester le renouvellement AUTOMATIQUE (fait par le CRON de virtualmin ?)

```shell
sudo certbot renew --dry-run
```
    
Documentation pour générer un certificat wildcard

    https://www.nikio.io/infrastructure/lets-encrypt-wildcard-certificate-with-certbot/
 
```shell
wget https://dl.google.com/go/go1.11.5.linux-amd64.tar.gz

# cd ~/
# tar -C /usr/local -xzf go1.11.5.linux-amd64.tar.gz
# export PATH=$PATH:/usr/local/go/bin
# go version
# go get github.com/joohoi/acme-dns/...
# rm go1.11.5.linux-amd64.tar.gz
# sudo mv go/bin/acme-dns /usr/local/bin/acme-dns
# rm -rf go
# which acme-dns
```   
  
 
Exemple de configuration (avec le module apache, elle est adaptée AUTOMATIQUEMENT)


Redémarrer apache
-----------------

```shell
service apache2 restart
```

Problèmes récurrents
--------------------

* *:80 ou *:443 ne marche pas mais IP_DU_SERVEUR:80 oui

* En cas de proxy, pour Jenkins par exemple, la version 443 redirige vers https://127.0.0.1:8080 au lieu de http://127.0.0.1:8080

    ```apacheconfig
    <VirtualHost *:80 *:443>                     						
            ServerAdmin admin@devinthehood.com
            ServerName www.devinthehood.com
            ServerAlias devinthehood.com               					
            DocumentRoot /home/devinthehood/public_html/server
            ErrorLog /home/devinthehood/logs/error.log
            CustomLog /home/devinthehood/logs/access.log combined
            
            SSLEngine on									
            SSLOptions +StrictRequire							
            SSLCertificateChainFile /etc/letsencrypt/live/devinthehood.com/chain.pem		
            SSLCertificateFile /etc/letsencrypt/live/devinthehood.com/cert.pem		
            SSLCertificateKeyFile /etc/letsencrypt/live/devinthehood.com/privkey.pem		
            RewriteEngine On								
            RewriteCond %{HTTPS} off							
            RewriteRule (.*) https://%{HTTP_HOST}%{REQUEST_URI}				
    </VirtualHost>
    ```
    
* Migrer un certificat d'un serveur nginx vers un serveur apache, ouvrir le fichier référencé en tant que SSLCertificateFile dans la conf nginx et le couper en deux, la partie certificat vers un fichier server.crt et la partie key vers un fichier server.key, référencer ensuite ces deux fichiers dans le fichier de conf apache du projet tel que:

```shell
<VirtualHost *:443>
	DocumentRoot ...
	ServerName ...

	<Directory ...
	</Directory>

	SSLEngine on
	SSLCertificateFile /path/to/server.crt
	SSLCertificateKeyFile /path/to/server.key
</VirtualHost>
```

License
-------

The Server Config is open-sourced software licensed under the [MIT license](https://opensource.org/licenses/MIT).

&copy; 2019 [dev in the hood](https://devinthehood.com)
