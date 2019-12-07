<p align="center">
    <a href="https://vsweb.be"><img src="https://vsweb.be/userfiles/images/14548837631453228685logo.png" alt="logo VsWeb"></a>
</p>

<p align="center">
    <a href="https://opensource.org/licenses/MIT" target="_blank"><img src="https://img.shields.io/badge/License-MIT-yellow.svg" alt="License"></a>
    <a href="https://github.com/jul6art/symfony-skeleton" target="_blank"><img src="https://img.shields.io/static/v1?label=stable&message=v1&color=success" alt="Version"></a>
</p>

Configuration de serveurs
=========================
Serveur de mails
----------------

LIENS POUR TESTER SERVEUR EMAIL
------------------------------

https://mxtoolbox.com

https://www.mail-tester.com/


ip: 212.47.251.31

reverse dns ipv4 : 31.1-24.251.47.212.in-addr.arpa

Ajouter un compte mail pour un domaine

    sql dans table postfix
    
```sql
INSERT INTO `domaines` ( `domaine` , `etat` ) VALUES ('VOTRE_DOMAINE.com', '1');
INSERT INTO `comptes` ( `email` , `password` , `quota` , `etat` , `imap` , `pop3` ) VALUES 
('contact@VOTRE_DOMAINE.com', ENCRYPT( 'VOTRE_PASS_MAIL' ) , '0', '1', '1', '1');
```
    
```console
telnet 127.0.0.1 25
ehlo domaine.extension
mail from: <adresse@domaine.exten>
rcpt to: <adresse@domaine.exten>
data
hello world
.
quit


newaliases
```


> Si erreurs, restarter les services ou voir plus bas

> Utiliser reverse dns ipv4 et ip ipv4  //corrige envoi sur tout fournisseur de messagerie

> Dans /etc/courier/authdaemonrc, mettre debug login à 2

> Redémarrer les /etc/init.d de courier et postfix regulierement

> Ajouter inet_protocols = ipv4 dans /etc/postfix/main.cf

> Ajouter "MYSQL_MAILDIR_FIELD     CONCAT(SUBSTRING_INDEX(email,'@',-1),'/',SUBSTRING_INDEX(email,'@',1),'/')"
dans le fichier /etc/courier/authmysqlrc 

> Pour pouvoir envoyer des mails automatiques, ajouter le domaine dans la table domaines de postfix

> Pour pouvoir ajouter un compte mail, AJOUTER DOMAINE DANS TABLE DOMAINE puis compte dans la table compte

```sql
INSERT INTO `comptes` ( `email` , `password` , `quota` , `etat` , `imap` , `pop3` ) VALUES 
('contact@VOTRE_DOMAINE.com', ENCRYPT( 'VOTRE_PASS_MAIL' ) , '0', '1', '1', '1');
```

Puis ajouter le domaine dans /etc/postfix/main.cf et creer le dossier du domaine dans /var/spool/vmail et corrige erreur de dossiers manquants dans /var/spool/vmail et 

    chown vmail:vmail DOMAINE -R     
    
Puis entrer dans le dossier et

    maildirmake NOMDUCOMPTE

Redémarrer les /etc/init.d de courier et postfix

Connaitre les champs MX

    host -t MX VOTRE_DOMAINE
    
Installer Postfix
 
    sudo apt-get install postfix-mysql   
    
> Chosisir "no configuration"


Dans nano /etc/postfix/master.cf

    # service type  private unpriv  chroot  wakeup  maxproc command + args
    #               (yes)   (yes)   (yes)   (never) (100)
    # ==========================================================================
    smtp      inet  n       -       n       -       -       smtpd

> Remplacez le n situé sous "chroot" par un tiret.

Sauvegarder et quitter

Créer les tables

    mysql -u root -p
    
```sql
CREATE USER 'postfix'@ 'localhost' IDENTIFIED BY 'VOTRE_PASS_POSTFIX_MYSQL';
GRANT USAGE ON * . * TO 'postfix'@'localhost' IDENTIFIED BY 'VOTRE_PASS_POSTFIX_MYSQL';
CREATE DATABASE `postfix` ;
GRANT ALL PRIVILEGES ON `postfix` . * TO 'postfix'@ 'localhost';
FLUSH PRIVILEGES;

USE postfix;

CREATE TABLE `domaines` (
`domaine` varchar(255) NOT NULL default '',
`etat` tinyint(1) NOT NULL default '1',
PRIMARY KEY  (`domaine`)
) ENGINE=MyISAM;

CREATE TABLE `comptes` (
`email` varchar(255) NOT NULL default '',
`password` varchar(255) NOT NULL default '',
`quota` int(10) NOT NULL default '0',
`etat` tinyint(1) NOT NULL default '1',
`imap` tinyint(1) NOT NULL default '1',
`pop3` tinyint(1) NOT NULL default '1',
PRIMARY KEY  (`email`)
) ENGINE=MyISAM;

CREATE TABLE `alias` (
`source` varchar(255) NOT NULL default '',
`destination` text NOT NULL,
`etat` tinyint(1) NOT NULL default '1',
PRIMARY KEY  (`source`)
) ENGINE=MyISAM;
```

 
Configuration de Postfix

Maintenant que nos tables MySQL sont pretes, nous allons pouvoir les renseigner dans les fichiers de configuration de postfix.

Nous allons créer un fichier de configuration par requêtes:

    Une requête pour récupérer les Domaines: mysql-virtual_domaines.cf
    Une requête pour récupérer les Comptes email: mysql-virtual_comptes.cf
    Une requête pour récupérer les Alias: mysql-virtual_aliases.cf
    Une requête pour récupérer les correspondances Alias -> Comptes Mails: mysql-virtual_aliases_comptes.cf
    Une requête pour récupérer les Quotas: mysql-virtual_quotas.cf

Tous ces fichiers seront stockés dans le répertoire /etc/postfix/

    nano /etc/postfix/mysql-virtual_domaines.cf

        hosts = 127.0.0.1
        user = postfix
        password = VOTRE_PASS_POSTFIX_MYSQL
        dbname = postfix
        select_field = 'virtual'
        table = domaines
        where_field = domaine
        additional_conditions = AND etat=1


    nano /etc/postfix/mysql-virtual_comptes.cf

        hosts = 127.0.0.1
        user = postfix
        password = VOTRE_PASS_POSTFIX_MYSQL
        dbname = postfix
        table = comptes
        select_field = CONCAT(SUBSTRING_INDEX(email,'@',-1),'/',SUBSTRING_INDEX(email,'@',1),'/')
        where_field = email
        additional_conditions = AND etat=1


    nano /etc/postfix/mysql-virtual_aliases.cf

        hosts = 127.0.0.1
        user = postfix
        password = VOTRE_PASS_POSTFIX_MYSQL
        dbname = postfix
        table = alias
        select_field = destination
        where_field = source
        additional_conditions = AND etat=1


    nano /etc/postfix/mysql-virtual_aliases_comptes.cf

        hosts = 127.0.0.1
        user = postfix
        password = VOTRE_PASS_POSTFIX_MYSQL
        dbname = postfix
        table = comptes
        select_field = email
        where_field = email
        additional_conditions = AND etat=1

 
    nano /etc/postfix/mysql-virtual_quotas.cf

        hosts = 127.0.0.1
        user = postfix
        password = VOTRE_PASS_POSTFIX_MYSQL
        dbname = postfix
        table = comptes
        select_field = quota
        where_field = email

Nos fichiers de configuration Postfix <-> Mysql sont prêts. Nous allons créer le fichier de configuration principal de postfix.

Tout d'abord, nous créons un groupe et un utilisateur mail virtuel:

    groupadd -g 5000 vmail
    useradd -g vmail -u 5000 vmail -d /var/spool/vmail/ -m

Ensuite, on créé le fichier de configuration principal:

    nano /etc/postfix/main.cf

        smtpd_banner = $myhostname ESMTP $mail_name (Debian/GNU)
        biff = no
        disable_vrfy_command = yes
        smtpd_helo_required = yes

        append_dot_mydomain = no

        myhostname = REVERSE_DNS
        myorigin = REVERSE_DNS
        mydestination = REVERSE_DNS, localhost.localdomain, localhost
        relayhost =
        mynetworks = 127.0.0.0/8, IP_PUBLIQUE_SERVEUR
        mailbox_size_limit = 0
        recipient_delimiter = +
        inet_interfaces = all
        inet_protocols = ipv4
        
        virtual_alias_maps = mysql:/etc/postfix/mysql-virtual_aliases.cf,mysql:/etc/postfix/mysql-virtual_aliases_comptes.cf
        virtual_mailbox_domains = mysql:/etc/postfix/mysql-virtual_domaines.cf
        virtual_mailbox_maps = mysql:/etc/postfix/mysql-virtual_comptes.cf
        virtual_mailbox_base = /var/spool/vmail/
        virtual_uid_maps = static:5000
        virtual_gid_maps = static:5000
        
        virtual_create_maildirsize = yes
        virtual_mailbox_extended = yes
        virtual_mailbox_limit_maps = mysql:/etc/postfix/mysql-virtual_quotas.cf
        virtual_mailbox_limit_override = yes
        virtual_maildir_limit_message = "La boite mail de votre destinataire est pleine, merci de reessayez plus tard."
        virtual_overquota_bounce = yes

        smtpd_sender_restrictions =
                permit_mynetworks,
                warn_if_reject reject_unverified_sender
        
        smtpd_recipient_restrictions =
                permit_mynetworks,
                reject_unauth_destination,
                reject_non_fqdn_recipient
        
        smtpd_client_restrictions =
                permit_mynetworks

Un peu de sécurité

Comme vous l'avez vu, le mot de passe MySQL de notre utilisateur postfix est en clair, il faut donc restreindre l'accès aux fichiers de conf à root et postfix uniquement.

    chmod u=rw,g=r,o= /etc/postfix/mysql-virtual_*.cf
    chgrp postfix /etc/postfix/mysql-virtual_*.cf
    l /etc/postfix/

        -rw-r--r--  1 root root      373 2009-01-09 11:40 dynamicmaps.cf
        -rw-r--r--  1 root root     2731 2009-01-09 15:15 main.cf
        -rw-r--r--  1 root root     3968 2009-01-09 11:40 master.cf
        -rw-r-----  1 root postfix   174 2009-01-09 13:02 mysql-virtual_aliases.cf
        -rw-r-----  1 root postfix   169 2009-01-09 13:02 mysql-virtual_aliases_comptes.cf
        -rw-r-----  1 root postfix   238 2009-01-09 13:01 mysql-virtual_comptes.cf
        -rw-r-----  1 root postfix   176 2009-01-09 13:01 mysql-virtual_domaines.cf
        -rw-r-----  1 root postfix   134 2009-01-09 13:02 mysql-virtual_quotas.cf
        -rw-r--r--  1 root root    17975 2008-08-19 07:51 postfix-files
        -rwxr-xr-x  1 root root     6840 2008-08-19 07:51 postfix-script
        -rwxr-xr-x  1 root root    22239 2008-08-19 07:51 post-install
        drwxr-xr-x  2 root root     4096 2008-08-19 07:51 sasl

Les droits ont bien été mis à jour.

On reboot postfix pour qu'il prenne en compte toutes nos modifications

    /etc/init.d/postfix restart

Et pour s'assurer qu'il n'y'a pas de problème de syntaxe dans notre configuration, tapez:

    postfix check
 
Test de Postfix

Dans un premier temps, insérez des données dans les tables MySQL postfix.

On insère un domaine valide qui sera géré par postfix:

```sql
INSERT INTO `domaines` ( `domaine` , `etat` ) VALUES ('VOTRE_DOMAINE.com', '1');
```

Et on créé un compte virtuel:

```sql
INSERT INTO `comptes` ( `email` , `password` , `quota` , `etat` , `imap` , `pop3` ) VALUES 
('contact@VOTRE_DOMAINE.com', ENCRYPT( 'VOTRE_PASS_MAIL' ) , '0', '1', '1', '1');
```

Nous allons voir si postfix fonctionne correctement, pour cela installez telnet (apt-get install telnet) si vous ne l'avez pas par défaut.

    telnet 127.0.0.1 25
        Trying 127.0.0.1...
        Connected to 127.0.0.1.
        Escape character is '^]'.
        220 mail.lafermeduweb.net ESMTP (Debian/GNU)
        ehlo VOTRE_DOMAINE
        250-mail.VOTRE_DOMAINE
        250-PIPELINING
        250-SIZE 10240000
        250-ETRN
        250-ENHANCEDSTATUSCODES
        250-8BITMIME
        250 DSN
        mail from: <postmaster@VOTRE_DOMAINE>
        250 2.1.0 Ok
        rcpt to: <contact@VOTRE_DOMAINE>
        250 2.1.5 Ok
        data
        354 End data with .
        Hello ca farte ?
        .
        250 2.0.0 Ok: queued as 286AA8C4089
        quit
        221 2.0.0 Bye

Nous avons envoyé un email en provenance de l'adresse test@VOTRE_DOMAINE vers notre compte mail récemment créé (contact@VOTRE_DOMAINE).

Jetons un oeil dans les logs générés:

    cat /var/log/mail.log
        Jan 12 14:28:50 F055 postfix/smtpd[27791]: connect from localhost[127.0.0.1]
        Jan 12 14:29:28 F055 postfix/smtpd[27791]: 78F628C4596: client=localhost[127.0.0.1]
        Jan 12 14:29:38 F055 postfix/cleanup[27796]: 78F628C4596: message-id=<20090112132928.
        78F628C4596@VOTRE_DOMAINE>
        Jan 12 14:29:38 F055 postfix/qmgr[26852]: 78F628C4596: from=, size=366, 
        nrcpt=1 (queue active)
        Jan 12 14:29:38 F055 postfix/virtual[27800]: 78F628C4596: to=, relay=virtual, 
        delay=22, delays=22/0.01/0/0.02, dsn=2.0.0, status=sent (delivered to maildir)
        Jan 12 14:29:38 F055 postfix/qmgr[26852]: 78F628C4596: removed
        Jan 12 14:29:40 F055 postfix/smtpd[27791]: disconnect from localhost[127.0.0.1]

"delivred to maildir" indique que notre mail est bien arrivé à destination!
Erreurs fréquentes:

Regardez vos logs (cat /var/log/mail.log) en cas d'erreur.

Feb  4 16:29:10 ns205674 postfix/smtpd[29555]: fatal: open database /etc/aliases.db: No such file or directory

Si vous obtenez cette erreur, tapez dans votre console:

    newaliases


Installation de courier avec support MySQL

    apt-get install courier-base courier-authdaemon courier-authlib-mysql courier-imap courier-pop

> Répondez No à la question sur la séparation des fichiers de configuration pour l'administration web.

 
Configuration de Courier

Nous allons déjà activer l'authentification Courier avec le module MySQL.

    nano /etc/courier/authdaemonrc

Modifiez la ligne 27:

    authmodulelist="authmysql"

Sauvegarder et quitter.

Comme pour Postfix, nous allons renseigner les champs et la table MySQL à Courier:

    nano /etc/courier/authmysqlrc

Modifiez les paramètres de cette façon:

    MYSQL_SERVER            localhost
    MYSQL_USERNAME          postfix
    MYSQL_PASSWORD          VOTRE_PASS_POSTFIX_MYSQL
    MYSQL_DATABASE          postfix
    MYSQL_USER_TABLE        comptes
    MYSQL_CRYPT_PWFIELD     password
    MYSQL_UID_FIELD         5000
    MYSQL_GID_FIELD         5000
    MYSQL_LOGIN_FIELD       email
    MYSQL_HOME_FIELD        "/var/spool/vmail/"

Sauvegarder et quitter.

Il faut maintenant rebooter les services de courier pour qu'il prenne en compte nos modifications:

    /etc/init.d/courier-authdaemon restart &&
    /etc/init.d/courier-imap restart &&
    /etc/init.d/courier-pop restart

Pour tester si tout fonctionne correctement, nous allons directement mettre en place un webmail sur notre serveur web, il peut s'avérer pratique pour consulter vos emails lorsque vous n'êtes pas sur votre machine.

Mise en place d'un webmail: Roundcube

RoundCube - Webmail

Téléchargez roundcube sur votre serveur: http://roundcube.net/downloads

    wget https://github.com/roundcube/roundcubemail/releases/download/1.2.4/roundcubemail-1.2.4-complete.tar.gz

    tar -zxvf roundcubemail-0.2-stable.tar.gz

    mv roundcubemail-0.2-stable/	 /var/www/webmail/

Votre webmail est en place, il faut maintenant le configurer.

    mysql -u root -p

```sql
CREATE DATABASE roundcubemail;
GRANT ALL PRIVILEGES ON roundcubemail.* TO roundcube@localhost  IDENTIFIED BY 'PASS_ROUNDCUBE';
quit
```

Notre base de données roundcubemail est créée, il faut maintenant importer les tables

    mysql -u root -p roundcubemail < /var/www/webmail/SQL/mysql.initial.sql

Lancez maintenant votre navigateur web et entrez l'adresse suivante:

> http://VOTRE_IP_DE_SERVEUR/webmail/installer/

Suivez pas à pas les instructions pour installer le webmail.

PLUGINS A INSTALLER

    $config['plugins'] = array('additional_message_headers', 'archive', 'attachment_reminder', 'autologon', 'emoticons', 
                'example_addressbook', 'filesystem_attachments', 'help', 'hide_blockquote', 'identity_select', 
                'jqueryui', 'legacy_browser', 'managesieve', 'markasjunk', 'new_user_dialog', 'new_user_identity', 
                'newmail_notifier', 'password', 'userinfo', 'zipdownload');

En fin d'installation, copiez les deux sources générées, et collez les dans les fichiers de conf du serveur:

    db.inc.php
    main.inc.php

Supprimez le répertoire installer/ une fois que vos fichiers de conf sont créés.

http://VOTRE_IP_DE_SERVEUR/webmail/

Et maintenant vous devriez pouvoir accéder au webmail.

Entrez le nom de votre compte précedemment inséré dans les tables de postfix (contact/password), et vous devriez pouvoir vous connecter si tout marche correctement.

Avec roundcube, vous pouvez envoyer et recevoir des messages avec votre adresse contact@VOTRE_DOMAINE.

> ! Si serveur chez scaleway, penser à activer le smtp dans la section security et si impossible de se connecter, verifier dans les logs qu il n y a pas une erreur dans la requete sql de login

sinon, AJOUTER UNE COLONNE A LA TABLE Comptes

```sql
ALTER TABLE `postfix`.`comptes` ADD COLUMN `name` VARCHAR(255) NULL DEFAULT NULL;
```

> DEBUG

ouvrir ports
-----------

    imap 143, 993
    smtp 25, 465
    pop3 110, 995

lister les ports ouverts
------------------------

    iptables -L

tester un ports
---------------

    telnet 127.0.0.1 port

    nano /etc/iptables.test.rules

    Allows SMTP access

        -t filter -A INPUT -p tcp --dport 25 -j ACCEPT

    Allows pop and pops connections
    
        -t filter -A INPUT -p tcp --dport 110 -j ACCEPT
        -t filter -A INPUT -p tcp --dport 995 -j ACCEPT

    Allows imap and imaps connections
    
        -t filter -A INPUT -p tcp --dport 143 -j ACCEPT
        -t filter -A INPUT -p tcp --dport 993 -j ACCEPT
	
    iptables-restore < /etc/iptables.test.rules

   

License
-------

The VsWeb Server Config is open-sourced software licensed under the [MIT license](https://opensource.org/licenses/MIT).

&copy; 2019 [VsWeb](https://vsweb.be) 

















