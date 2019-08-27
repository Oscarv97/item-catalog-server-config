# Udacity - Linux Server Configuration

* Oscar Vial , Project  5 of Udacity Fullstack developer nanodegree

A README file is included in the GitHub repo containing the following information: IP address, URL, summary of software installed, summary of configurations made, and a list of third-party resources used to complete this project.

## IP & Hostname

 - IP: 54.82.99.198/
 - Url http://ec2-54-82-99-198.compute-1.amazonaws.com


## Summary of software install

* python pip
* pyhton 3 pip
* apache2
* git
* -Flask==1.1.1
* SQLAlchemy==1.3.7
* requests-toolbelt==0.8.0
* google-auth==1.4.1
* requests==2.22.0
* flask-cors==3.0.3
* Fail2Ban
* virtual enviroment enve3
* Latest Ubuntu packages and updates

## Steps taken for setup

* provisioned Amazon light sail server
* Added user `grader` 
* set user password
* add user to sudors
* Disable ssh login for root user
* configured and enanled`ufw`
* set HTTP to 80
* set UDP to 123
* Installed Apache
* Installed `sudo apt-get install libapache2-mod-wsgi-py3` 
* Cloned the item catalog project
* installed the project dependencices and set up dataBase (SQlLite)
* Created and configured the `catalog.wsgi` file for apache to serve the application


``` python
    #!/usr/bin/python
activate_this = '/var/www/catalog/venv/bin/activate_this.py'
with open(activate_this) as file_:
    exec(file_.read(), dict(__file__=activate_this))
    
import sys
import logging
logging.basicConfig(stream=sys.stderr)
sys.path.insert(0,"/var/www/catalog/")

from server import app as application
```

* Changed the default apache host file to 
``` xml
    <VirtualHost *:80>
        ServerName 107.21.134.50
        ServerAlias ec2-107-21-134-50.compute-1.amazonaws.com
        ServerAdmin admin@107.21.134.50
        WSGIDaemonProcess catalog python-path=/var/www/catalog:/var/www/catalog/venv/lib/python2.7/site-packages
        WSGIProcessGroup catalog
        WSGIScriptAlias / /var/www/catalog/catalog.wsgi
        <Directory /var/www/catalog/catalog/>
            Order allow,deny
            Allow from all
        </Directory>
        Alias /static /var/www/catalog/catalog/server/public/js/
        <Directory /var/www/catalog/catalog/server/public/js/>
            Order allow,deny
            Allow from all
        </Directory>
        ErrorLog ${APACHE_LOG_DIR}/error.log
        LogLevel warn
        CustomLog ${APACHE_LOG_DIR}/access.log combined
    </VirtualHost>
```

* Restarted Apache `sudo service apache2 restart`

* Site can be viewed [here](http://54.82.99.198/#/home)

## Notes
* I changed my database structure prior to submission of Project 4 and descovered that i had mixed python version, I was unbale to populate the data on the server , but it responds. That project should no hinder the criteria of this 

## Resources used 
[How to make proper ssh keys](https://www.digitalocean.com/community/questions/ubuntu-16-04-creating-new-user-and-adding-ssh-keys)

[Helpfull basic guide that was losely followed for apache and python](https://medium.com/@esteininger/python-3-5-flask-apache2-mod-wsgi3-on-ubuntu-16-04-67894abf9f70)

