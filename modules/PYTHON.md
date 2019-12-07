<p align="center">
    <a href="https://vsweb.be"><img src="https://vsweb.be/userfiles/images/14548837631453228685logo.png" alt="logo VsWeb"></a>
</p>

<p align="center">
    <a href="https://opensource.org/licenses/MIT" target="_blank"><img src="https://img.shields.io/badge/License-MIT-yellow.svg" alt="License"></a>
    <a href="https://github.com/jul6art/symfony-skeleton" target="_blank"><img src="https://img.shields.io/static/v1?label=stable&message=v1&color=success" alt="Version"></a>
</p>

Configuration de serveurs
=========================
Python
------

![logo Python](https://www.python.org/static/community_logos/python-logo-master-v3-TM.png "logo python")

Installation (SAUF POUR UN PROJET DJANGO)
-----------------------------------------

```console
sudo apt-get install python3-pip python3-dev python3-setuptools libjpeg-dev -y &&
sudo apt-get install libtiff5-dev libjpeg62-turbo-dev zlib1g-dev libfreetype6-dev liblcms2-dev libwebp-dev tcl8.6-dev tk8.6-dev python-tk -y &&
sudo apt-get install libapache2-mod-wsgi-py3 python3-mysqldb default-libmysqlclient-dev -y && 
pip3 install virtualenv mysqlclient
```
    
Déploiement d'un projet Django
------------------------------

```console
cd DOSSIER 

python3 -m virtualenv myprojectenv &&
source myprojectenv/bin/activate &&
which pip3

pip3 install -r requirements.txt 

mysql -u root

python3 manage.py makemigrations
python3 manage.py migrate

python3 manage.py collectstatic

python3 manage.py runserver 0.0.0.0:8000

deactivate
```
    
Créer vhost

```apacheconfig
<VirtualHost *:80>
        ServerAdmin admin@vsweb.be
        ServerName lauto-resultat.com
        ServerAlias www.lauto-resultat.com
        ErrorLog /var/www/html/logs/error.log
        CustomLog /var/www/html/logs/access.log combined
        DocumentRoot /var/www/html/auto_resultat/
        Alias /media /var/www/html/auto_resultat/media
        Alias /static /var/www/html/auto_resultat/static
    <Directory /var/www/html/auto_resultat/static>
        Require all granted
    </Directory>

    <Directory /var/www/html/auto_resultat/my_app>
        <Files wsgi.py>
            Require all granted
        </Files>
    </Directory>


    WSGIDaemonProcess auto_resultat python-path=/var/www/html/auto_resultat:/var/www/html/auto_resultat/myprojectenv/lib/python3.4/site-packages
    WSGIProcessGroup auto_resultat
    WSGIScriptAlias / /var/www/html/auto_resultat/my_app/wsgi.py
</VirtualHost>
```

Nouvelles commandes

```console
virtualenv myprojectenv &&
source myprojectenv/bin/activate &&
apt-get install libjpeg-dev &&
pip install pillow django==1.8.12 &&
pip install pip --upgrade
apt-get build-dep python-mysqldb
pip install MySQL-python
nano auto_resultat/settings.py
```
    
Paramètres

```console
clef:  x2_i^0(a@wx7jqa0c=ub1@9*fmyx0njf@+_vadsvmywz+jm(2l debug false SERVER_EMAIL

DATABASES = {
    'default': {
            'ENGINE': 'django.db.backends.mysql', # Add 'postgresql_psycopg2', 'mysql', 'sqlite3' or 'oracle'.
            'NAME': 'lautoresultat',                      # Or path to database file if using sqlite3.
            # The following settings are not used with sqlite3:
            'USER': 'root',
            'PASSWORD': 'PASSWORD',
            'HOST': 'localhost',                      # Empty for localhost through domain sockets or           '127.0.0.1' for localhost through TCP.
            'PORT': '3326',                      # Set to empty string for default.
        }
    }

EMAIL_BACKEND = 'django.core.mail.backends.smtp.EmailBackend'
# EMAIL_SERVER = 'no_reply@lauto-resultat.com' 
EMAIL_HOST = 'smtp.lauto-resultat.com'
EMAIL_PORT = 25
EMAIL_HOST_USER = ''
EMAIL_HOST_PASSWORD = ''
EMAIL_USE_TLS = False
DEFAULT_FROM_EMAIL = 'Auto-resultat mail automatique <no_reply@lauto-resultat.com>'

STATIC_ROOT = os.path.join(BASE_DIR, "static/")
```

Nouvelles commandes

```console
python manage.py makemigrations  &&    
python manage.py migrate     
```               
    
> Si marche pas, python manage.py migrate --fake

```console
python manage.py collectstatic &&
python manage.py runserver 0.0.0.0:8000

deactivate   

rm /var/www/html/auto_resultat/db.sqlite3 &&
chown iroom:iroom /var/www/html/auto_resultat -R &&
chown :www-data /var/www/html/auto_resultat &&
chown :www-data /var/www/html/auto_resultat/myprojectenv &&
mkdir /var/www/html/logs &&
nano /etc/apache2/sites-available/000-default.conf
```

> peut corriger certaines erreurs     

```console
pip install -r requirements.txt
```

Redémarrer apache

```console
service apache2 restart
```

> En cas de probleme a la migration regarder LE NOM DE LA MIGRATION QUI FOIRE ET LA LIRE

> Modifier les fichiers models et views de simple-forums   et admin.py recopier cela: fichiers joints


License
-------

The VsWeb Server Config is open-sourced software licensed under the [MIT license](https://opensource.org/licenses/MIT).

&copy; 2019 [VsWeb](https://vsweb.be)
