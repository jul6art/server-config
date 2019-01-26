Configuration de serveurs
==
Enregistrements DNS
-
### [&#9756; Retour au menu](../README.md)
Enregistrements a

    IP.DU.SERVEUR       .domaine.ext.
    IP.DU.SERVEUR       *.domaine.ext.
    IP.DU.SERVEUR       mail.domaine.ext.
    s
Enregistrements aaaa

    ipv6                .domaine.ext.
    ipv6                *.domaine.ext.
    
Enregistrement mx

    priorite 10         .domaine.ext.       mail.domaine.ext. 
    
Enregistrement txt
> De type spf mais c'est un enregistrement txt il spécifie avec le "a" et le "mx" qu'il n'autorise les mails que pour les MXers du domaine qui ont la même ip que le champ A

    v=spf1 a mx ~all
    
> Vérifier dans zone DNS si les "" ont bien été ajoutés: v=spf1 a mx ~all  => "v=spf1 a mx ~all"




&copy; 2019 [VsWeb](https://vsweb.be)