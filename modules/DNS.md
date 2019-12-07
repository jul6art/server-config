<p align="center">
    <a href="https://vsweb.be"><img src="https://vsweb.be/userfiles/images/14548837631453228685logo.png" alt="logo VsWeb"></a>
</p>

<p align="center">
    <a href="https://opensource.org/licenses/MIT" target="_blank"><img src="https://img.shields.io/badge/License-MIT-yellow.svg" alt="License"></a>
    <a href="https://github.com/jul6art/symfony-skeleton" target="_blank"><img src="https://img.shields.io/static/v1?label=stable&message=v1&color=success" alt="Version"></a>
</p>

Configuration de serveurs
=========================
Enregistrements DNS
-------------------

Enregistrements a

    IP.DU.SERVEUR       .domaine.ext.
    IP.DU.SERVEUR       *.domaine.ext.
    IP.DU.SERVEUR       mail.domaine.ext.
    
Enregistrements aaaa

    ipv6                .domaine.ext.
    ipv6                *.domaine.ext.
    
Enregistrement mx

    priorite 10         .domaine.ext.       mail.domaine.ext. 
    
Enregistrement txt

> :warning: De type spf mais c'est un enregistrement txt il spécifie avec le "a" et le "mx" qu'il n'autorise les mails que pour les MXers du domaine qui ont la même ip que le champ A

    v=spf1 a mx ~all
    
> :warning: Vérifier dans zone DNS si les "" ont bien été ajoutés: v=spf1 a mx ~all  => "v=spf1 a mx ~all"


License
-------

The VsWeb Server Config is open-sourced software licensed under the [MIT license](https://opensource.org/licenses/MIT).

&copy; 2019 [VsWeb](https://vsweb.be)