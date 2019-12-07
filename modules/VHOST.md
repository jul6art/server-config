<p align="center">
    <a href="https://vsweb.be"><img src="https://vsweb.be/userfiles/images/14548837631453228685logo.png" alt="logo VsWeb"></a>
</p>

<p align="center">
    <a href="https://opensource.org/licenses/MIT" target="_blank"><img src="https://img.shields.io/badge/License-MIT-yellow.svg" alt="License"></a>
    <a href="https://github.com/jul6art/symfony-skeleton" target="_blank"><img src="https://img.shields.io/static/v1?label=stable&message=v1&color=success" alt="Version"></a>
</p>

Configuration de serveurs
=========================
Modèles de fichiers de conf Apache
----------------------------------

> Sélectionner "add server templates to virtualmin from default"

Default settings SSL

```apacheconfig
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
```

Symfony Template

```apacheconfig
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
```

Symfony Template SSL

```apacheconfig
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
```

Django template

```apacheconfig
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
```

Django template SSL
    
```apacheconfig
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
```


License
-------

The VsWeb Server Config is open-sourced software licensed under the [MIT license](https://opensource.org/licenses/MIT).

&copy; 2019 [VsWeb](https://vsweb.be)