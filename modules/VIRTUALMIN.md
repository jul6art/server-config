Configuration de serveurs
==
Installation Virtualmin
-
### [&#9756; Retour au menu](../README.md)

########################################################################
############################STEP 2#######################################
########################################################################
# install minimal configuration
apt-get update -y && apt-get install build-essential sudo fail2ban git -y &&
cd /home &&
wget https://files.phpmyadmin.net/phpMyAdmin/4.6.0/phpMyAdmin-4.6.0-all-languages.tar.gz && 
tar xzf phpMyAdmin-4.6.0-all-languages.tar.gz &&
rm phpMyAdmin-4.6.0-all-languages.tar.gz &&
mv phpMyAdmin-4.6.0-all-languages phpmyadmin &&

############ OPTIONNAL IF U WANT PYTHON PROJETCS #########################
apt-get install python3-pip python3-dev python3-setuptools libjpeg-dev -y &&
apt-get install libtiff5-dev libjpeg62-turbo-dev zlib1g-dev libfreetype6-dev liblcms2-dev libwebp-dev tcl8.6-dev tk8.6-dev python-tk -y &&
apt-get install libapache2-mod-wsgi-py3 -y && 
pip3 install virtualenv && 


########################################################################
############################STEP 3#######################################
########################################################################
# copy the virtualmin.sh script
nano install.sh
# OR
# wget http://software.virtualmin.com/gpl/scripts/install.sh

########################################################################
############################STEP 4#######################################
########################################################################
# run the script
source install.sh 
# y/n -> y et hostname défini: vsweb03.vsweb.be      (important pour configuration postfix)


########################################################################
############################STEP 7#######################################
########################################################################
# add server templates to virtualmin from default
# Default settings SSL
ServerName ${DOM}
ServerAlias www.${DOM}
RedirectMatch ^/(.*)$ https://${DOM}/$1
DocumentRoot ${HOME}/public_html
ErrorLog /var/log/virtualmin/${DOM}_error_log
CustomLog /var/log/virtualmin/${DOM}_access_log combined
ScriptAlias /cgi-bin/ ${HOME}/cgi-bin/
DirectoryIndex index.html index.htm index.php index.php4 index.php5
<Directory ${HOME}/public_html>
	Options -Indexes +IncludesNOEXEC +SymLinksIfOwnerMatch
	allow from all
	AllowOverride All Options=ExecCGI,Includes,IncludesNOEXEC,Indexes,MultiViews,SymLinksIfOwnerMatch
</Directory>
<Directory ${HOME}/cgi-bin>
	allow from all
	AllowOverride All Options=ExecCGI,Includes,IncludesNOEXEC,Indexes,MultiViews,SymLinksIfOwnerMatch
</Directory>

# Symfony Template
ServerName www.${DOM}
ServerAlias ${DOM}
DocumentRoot ${HOME}/public_html/web
Alias /${HOME} /home/${HOME}/public_html/web/
ErrorLog /var/log/virtualmin/${DOM}_error_log
CustomLog /var/log/virtualmin/${DOM}_access_log combined
ScriptAlias /cgi-bin/ ${HOME}/cgi-bin/
<Directory ${HOME}/public_html/web>
	DirectoryIndex app.php
	Options -Indexes +IncludesNOEXEC +SymLinksIfOwnerMatch
	allow from all
	AllowOverride All Options=ExecCGI,Includes,IncludesNOEXEC,Indexes,MultiViews,SymLinksIfOwnerMatch
</Directory>
<Directory ${HOME}/cgi-bin>
	allow from all
	AllowOverride All Options=ExecCGI,Includes,IncludesNOEXEC,Indexes,MultiViews,SymLinksIfOwnerMatch
</Directory>	

# Symfony Template SSL
ServerName www.${DOM}
ServerAlias ${DOM}
RedirectMatch ^/(.*)$ https://${DOM}/$1
DocumentRoot ${HOME}/public_html/web
Alias /${HOME} /home/${HOME}/public_html/web/
ErrorLog /var/log/virtualmin/${DOM}_error_log
CustomLog /var/log/virtualmin/${DOM}_access_log combined
ScriptAlias /cgi-bin/ ${HOME}/cgi-bin/
<Directory ${HOME}/public_html/web>
	DirectoryIndex app.php
	Options -Indexes +IncludesNOEXEC +SymLinksIfOwnerMatch
	allow from all
	AllowOverride All Options=ExecCGI,Includes,IncludesNOEXEC,Indexes,MultiViews,SymLinksIfOwnerMatch
</Directory>
<Directory ${HOME}/cgi-bin>
	allow from all
	AllowOverride All Options=ExecCGI,Includes,IncludesNOEXEC,Indexes,MultiViews,SymLinksIfOwnerMatch
</Directory>				

# Django template
ServerName ${DOM}
ServerAlias www.${DOM}
ErrorLog /var/log/virtualmin/${DOM}_error_log
CustomLog /var/log/virtualmin/${DOM}_access_log combined
DocumentRoot ${HOME}/public_html
Alias /media ${HOME}/public_html/media
Alias /static ${HOME}/public_html/static
<Directory ${HOME}/public_html/media>
	Require all granted
</Directory>
<Directory ${HOME}/public_html/static>
	Require all granted
</Directory>
<Directory ${HOME}/public_html/my_app>
	<Files wsgi.py>
		Require all granted
	</Files>
</Directory>
WSGIDaemonProcess myownproject python-path=${HOME}/public_html:${HOME}/public_html/myprojectenv/lib/python3.4/site-packages
WSGIProcessGroup myownproject
WSGIScriptAlias / ${HOME}/public_html/my_app/wsgi.py

# Django template SSL
ServerName ${DOM}
ServerAlias www.${DOM}
RedirectMatch ^/(.*)$ https://${DOM}/$1
ErrorLog /var/log/virtualmin/${DOM}_error_log
CustomLog /var/log/virtualmin/${DOM}_access_log combined
DocumentRoot ${HOME}/public_html
Alias /media ${HOME}/public_html/media
Alias /static ${HOME}/public_html/static
<Directory ${HOME}/public_html/media>
	Require all granted
</Directory>
<Directory ${HOME}/public_html/static>
	Require all granted
</Directory>
<Directory ${HOME}/public_html/my_app>
	<Files wsgi.py>
		Require all granted
	</Files>
</Directory>
WSGIDaemonProcess myownproject python-path=${HOME}/public_html:${HOME}/public_html/myprojectenv/lib/python3.4/site-packages
WSGIProcessGroup myownproject
WSGIScriptAlias / ${HOME}/public_html/my_app/wsgi.py

########################################################################
############################STEP 8#######################################
########################################################################
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

########################################################################
############################STEP 9########################################
########################################################################
# dns record a
ip.ip.ip.ip .domaine.ext.
ip.ip.ip.ip *.domaine.ext.
ip.ip.ip.ip mail.domaine.ext.
# dns record aaaa
ipv6 .domaine.ext.
ipv6 *.domaine.ext.
# dns record mx
priorite 10 .domaine.ext. mail.domaine.ext. 
#dns record txt    (de type spf mais c'est un enregistrement txt il spécifie avec le "a" et le "mx" qu'il n'autorise les mails que pour les MXers du domaine qui ont la même ip que le champ A)
v=spf1 a mx ~all   (vérifier dans zone DNS si les "" ont bien été ajoutés: v=spf1 a mx ~all  => "v=spf1 a mx ~all")

########################################################################
############################STEP 10#######################################
########################################################################
# ajouter un serveur (domaine) sans se tracasser de l'ip qui ne ressemble pas à celle du serveur

# configurez le mail pour ce serveur (domaine)
Server Configuration -> Email Settings , and change Send outgoing email for domain from IP to Virtual server s address

# pour webmail username = user.domaine ex: admin.iroom

# pour thunderbird 
# laisser config autodetectee mais changer l IDENTIFIANT
identifiant						admin.iroom
nom complet					Mon nom complet
adresse email					admin@iroom.re
mot de passe					rmhacker6333

########################################################################
############################STEP 11#######################################
########################################################################
# SSL
IF BUY SSL COMODO
		# generer les fichiers key et csr et copier le contenu du fichier CSR généré sur https://ssls.com
		# nom de domaine = COMMON NAME (sans www)
		mkdir /home/ssl && openssl req -new -newkey rsa:2048 -nodes -keyout /home/ssl/iroom.key -out /home/ssl/iroom.csr
		mkdir /home/ssl && openssl req -new -newkey rsa:2048 -nodes -keyout /home/ssl/lauto-resultat.key -out /home/ssl/lauto-resultat.csr

		# Server Configuration
		# Manage SSL Certificate
		# New certificate
		put certificate text and PRIVATE KEY TEXT 
		active the certificate for usermin and webmin
ELSE
		# Manage SSL Certificate
		# Let's ENCRYPT
		# add all subdomains (without and with WWW) but NOT *.domaine.ext
		
# PUIS MODIFIER le fichier de conf
nano /etc/sites-enabled/site.conf
# modifier le virtualHost :443, changer ip.ip.ip.ip:443 par *:443
# supprimer le RedirectMatch qui est dans le virtual host :443
# SINON BOUCLE INFINIE

# puis restart apache
service apache2 restart






&copy; 2019 [VsWeb](https://vsweb.be)