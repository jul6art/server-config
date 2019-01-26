Configuration de serveurs
==
Modèles de fichiers de conf Apache
-
### [&#9756; Retour au menu](../README.md)
> Sélectionner "add server templates to virtualmin from default"

Default settings SSL

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

Symfony Template

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

Symfony Template SSL

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

Django template

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

Django template SSL
    
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

&copy; 2019 [VsWeb](https://vsweb.be)