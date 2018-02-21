# linux-server-configuration
Secure linux server on AWS

Your README.md file should include all of the following:
i. The IP address and SSH port so your server can be accessed by the reviewer.
ii. The complete URL to your hosted web application.
iii. A summary of software you installed and configuration changes made.
iv. A list of any third-party resources you made use of to complete this project.

Locate the SSH key you created for the grader user.

During the submission process, paste the contents of the grader user's SSH key into the "Notes to Reviewer" field

# Server Details
IP address: http://54.200.114.43/

SSH PORT: 2200

URL: http://ec2-54-200-114-43.us-west-2.compute.amazonaws.com

#Software Installed
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
in ic run sudo git clone 


# Steps
1. Are the applications up-to-date?
sudo apt-get update - latest infor stored in repos
sudo apt-get upgrade - upgrades installed packages
All system packages have been updated to most recent versions.
sudo apt-get install finger
sudo adduser grader
finger grader to see if created
2. Added user grader as sudo
3. Set up SSH keys
