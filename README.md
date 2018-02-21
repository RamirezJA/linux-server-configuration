# linux-server-configuration

# Server Details
IP address: http://54.200.114.43/

SSH PORT: 2200

URL: http://ec2-54-200-114-43.us-west-2.compute.amazonaws.com

# Software Installed

Flask

Apache

SqlAlchemy

Psychopg2

pip

git

Python

virtualenv

httplib2

# Configuration changes

# Update applications
sudo apt-get update

sudo apt-get upgrade

# Connecting to the instance from terminal(local)
Download the private key provided by Amazon lightsail from the accounts page


Put the text from the file in a file titled lightsail_key.rsa & place in ~/.ssh/


Then use chmod 600 ~/ssh/lightsail_key.rsa


Type ssh -i ~/.ssh/lightsail_key.rsa -p 2200 ubuntu@54.200.114.43 in order to log in from terminal


Using 2200 because that is the port that is being allowed due to the security changes made below
# Configure Firewall(UFW)
Run sudo ufw status to check status

sudo ufw default deny incoming blocks incoming connections on all ports

sudo ufw default allow incoming allows outgoing connections on all ports

sudo ufw allow 2200/tcp allows connections on port 2200

sudo ufw allow www allows connections on HTTP on port 80

sudo ufw allow ntp allows NTP connection on port 123

Then run sudo ufw show added in order to check the rules

sudo ufw enable to enable the firewall

Finally run sudo ufw status to check the status of the firewall

# User Management
create user grader using sudo adduser grader

create password:password

use su - grader to switch users

give grader sudo permission by using sudo visudo and add grader ALL=(ALL:ALL) ALL below root ALL=(ALL:ALL) ALL

save and exit

# Use SSH to let grader log in
run ssh-keygen on local machine

name generated key pair

switch to VM

using graders home directory create .ssh directory

run touch .ssh/authorized_keys

back on local machine run cat ~/.ssh/your-file-name.pub

copy contents to .ssh/authorized_keys on VM

run chmod 700 .ssh and chmod 644 .ssh/authorized_keys on VM

Force key based authentication logged in as grader access the /etc/ssh/sshd_config file and change 

PasswordAuthentication yes to no

save and exit then run sudo service ssh restart

Use ssh -i ~/.ssh/g_key -p 2200 grader@54.200.114.43 to log in as grader

the password is password

# Installing Apache & mod-wsgi
To install Apache sudo apt-get install apache2

To install mod-wsgi use sudo apt-get install libapache2-mod-wsgi python-dev

run sudo a2enmod wsgi to check that its running

# Installing PostgreSQL
Use sudo apt-get install postgresql to install PostgreSQL

Create a catalog user with limited permission

switch to user postgres with sudo su - postgres and type psql

run CREATE ROLE catalog WITH LOGIN; to create user

run ALTER ROLE catalog CREATEDB; to allow user to create database

run \password catalog to give user password

exit with \q then exit

# Create Catalog user in Linux
use sudo adduser catalog

password is password

give sudo by running sudo visudo under root add catalog ALL=(ALL:ALL) ALL

as catalog user run createdb catalog

# Adding project
install git using sudo apt-get install git

create directory ic in /var/www/ 

in ic run sudo git clone https://github.com/RamirezJA/ItemCatalogApp.git

change ownership of directory by running sudo chown -R ubuntu:ubuntu ic/

rename final.py by using mv final.py __init__.py & change app.run(host=...,port=...) to app.run()

# Oauth
Go to Google Developer Console and create a project

Create oauth client id and add the ip address and url to javascript origins

add http://ec2-54-200-114-43.us-west-2.compute.amazonaws.com/login,http://ec2-54-200-114-43.us-west-

2.compute.amazonaws.com/gconnect,http://ec2-54-200-114-43.us-west-2.compute.amazonaws.com/oauth2callback as authorized 

redirect 

download the json file and copy contents

paste into client_secrets.json in icapp folder
# Creat virt env
install pip run sudo apt-get install python-pip

install virtualenv sudo apt-get install python-virtualenv

in icapp run virtualenv venv

activate . venv/bin/activate

Next run: 

pip install --upgrade oauth2client

pip install httplib2

pip install psycopg2

pip install flask

pip install requests

pip install sqlalchemy

sudo apt-get install libpq-dev

Deactivate with deactivate

# Virtual Host
create a file in /etc/apache2/sites-available/ called icapp.conf
Add below:
```
<VirtualHost *:80>
  ServerName 54.200.114.43
  ServerAdmin dredhge@gmail.com
  WSGIScriptAlias / /var/www/ic/icapp.wsgi
  <Directory /var/www/ic/icapp/>
  Order allow,deny
  Allow from all
  Options -Indexes
  </Directory>
  Alias /static /var/www/ic/icapp/static
  <Directory /var/www/ic/icapp/static/>
  Order allow,deny
  Allow from all
  Options -Indexes
  </Directory>
  ErrorLog ${APACHE_LOG_DIR}/error.log
  LogLevel warn
  CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>

```
run sudo a2ensite ic

Next run sudo service apache2 reload

# Create WSGI file
In /var/www/ic run sudo nano icapp.wsgi

Then add:
```
activate_this = '/var/www/ic/icapp/venv/bin/activate_this.py'
execfile(activate_this, dict(__file__=activate_this))

#!/usr/bin/python
import sys
import logging
logging.basicConfig(stream=sys.stderr)
sys.path.insert(0,"/var/www/ic/")

from icapp import app as application
application.secret_key = 'super_secret_key'

```
next run sudo service apache2 restart

next change ownership by running sudo chown -R www-data:www-data ic/

# Database
In /var/www/ic/icapp/ run . venv/bin/activate

run nintendos.py

run deactivate

Finally run sudo service apache2 restart

Switch to browser and use ip address or url to access Item Catalog



# Sources
StackOverFlow 

https://www.digitalocean.com/community/tutorials/how-to-install-linux-apache-mysql-php-lamp-stack-on-ubuntu-14-04

https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/SSL-on-an-instance.html

https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/managing-users.html
