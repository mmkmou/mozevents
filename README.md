## Mozevent (mozilla events)

This django app allows you to set up an event system for you meetings, talks, parties...

## License

Check LICENSE file

### Installation

Clone this repo

```
git clone git@github.com:mozillahispano/mozevents.git
```

Create and fill your local settings (database, email, url and recaptcha)

```
cp settings_local.py.example settings_local.py
vim settings_local.py
```

Fill the initial database

```
python manage.py syndb
```

Run the testserver

```
python manage.py runserver
```

#### Apache

If you want to run on Apache2 server, you will have to install libapache2-mod-wsgi module and create a virtual host.

Here it's an example, change information if needed:

```
<VirtualHost *:80>
        DocumentRoot /var/lib/mozevents/public
        ServerAdmin admin@email.com
        ServerName admin@email.com
        ErrorLog /var/log/apache2/eventos.mozilla-hispano.org.error.log
        CustomLog /var/log/apache2/eventos.mozilla-hispano.org.log combined

	# Django settings
    	WSGIScriptAlias / /var/lib/mozevents/public/wsgi_handler.py
    	WSGIDaemonProcess mozevents user=www-data group=www-data processes=2 threads=5 maximum-requests=100
    	WSGIProcessGroup mozevents

   	<Directory /var/lib/mozevents/public>
        	 Order deny,allow
         	Allow from all
   	</Directory>

    	# Non-Django directories
    	Alias /static /var/lib/mozevents/public/static/
    	<Location "/static">
        	SetHandler None
    	</Location>
 
	Alias /media/ /var/lib/mozevents/public/media/

	# Directory protection
	<Directory /var/lib/mozevents/public/media/>
                Options -Indexes
		Order deny,allow
                Allow from all
        </Directory>

	Alias /admin-media /usr/share/pyshared/django/contrib/admin/media
   
	<Directory /usr/share/pyshared/django/contrib/admin/media/>
        	 Order deny,allow
         	Allow from all
   	</Directory>

</VirtualHost>
```

If you application is not under /var/lib/mozevents, you will have also to modify 
/public/wsgi_handler.py to change the path.

Check also if the python-django path is correct for your system.