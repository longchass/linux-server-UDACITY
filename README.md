# FULLSTACK NANODEGREE-UDACITY
#Server details
- IP address:54.252.223.127
- SSH port: 2200
- Hostname:ec2-54-252-223-127.ap-southeast-2.compute.amazonaws.com

#Description

This is the last project for udacity fullstack nanodegree
In this project, you need to use lightsail, a part of amazon web service(AWS) to create a virtual machine and prepare it to host your application
like the item catalog that you have created before. Since the project requirement does not tell me to do that you will only be welcomed with 
apache default page upon accessing the ip address

#Setup
1. Make an AWS account
side notes: even though the udacity instruction is simple but sigining up for an AWS has become harder and more complicated in the recent time
but i asure that after creating an account everything will work properly
2. Using [lightsail](lghtsail.aws.amazon.com) create an instance with OS only using ubuntu with the cheapest plan (will work for this project)
3. Get an ssh key from lightsail of your instance to get access later on the file type is ```.pem```
#Notes
1. If you want access to the instance on your terminal you need to have a bash terminal
on Mac and linux should be fine but on Windows you need to download one or have another virutal machine on your computer. The easiest way would be downloading [git bash](lightsail.aws.amazon.com) for a terminal
2. If you are testing if your server is online or not by entering the https://(pin)
i would suggest using firefox to do that using the default google chrome or canary would not get you the result you are expecting

#Steps (can be done without the first 3 steps)
1. Download private key and move it into your .ssh folder
2. Use chmod 400 to secure your folder access ```~/.ssh/yourdownloadekeyname.pem```
3. Login from your local machine's terminal using ```ssh -i ~/.ssh/yourdownloadedkeyname.pem intanceusername@youripaddress```
4. update the apt with new files ```sudo apt-get update```
5. update the exsit files within apt ```sudo apt-get upgrade```
6. Create a user name grader ```sudo adduser grader```
7. Give grader ```sudo``` access ```sudo vi(nano) /etc/sudoers.d/grader``` edit and type ```grader ALL=(ALL:ALL) NOPASSWD:ALL```
from here we are trying to make grader accessible from the terminal of your local machine(computer)
8. generate a key from your local machine ```ssh-keygen``` choose the file you want to save in (must be .ssh)
9. in your virutal machine logged in as grader ```su-grader``` make a .ssh directory and ```.ssh/authorized_keys``` and copy paste content from the file have ```.pub```
(do not forget to restrict access of the .ssh and the authorized_keys on your virtual machine)
10. update the ssh using ```service ssh restart```
11. now access it from the terminal on your local machine using ```ssh -i ~/.ssh/[filename without the .pub extension] grader@youripaddress```
from now on everything should take place in your ```grader``` on the virtual machine

#Steps(on your virtual machine user grader)
1. Setup firewalls and allow it on port ```2200``` , ```123```, ```80```
```sudo ufw allow 2200/tcp```
```sudo ufw allow 123/ntp```
```sudo ufw allow 80/tcp```
2. install apache, postgreSQl. wsgi
```sudo apt-get install apache2```
```sudo apt-get isntall postgresql postgresql-contrib```
```sudo apt-get install libapache2-mod-wsgi```
Finish

#Steps(for the app)

1. ```cd var/www```
2. ```sudo mkdir catalog ```cd catalog
3. ```create a catalog.wsgi file and paste this
import sys
import logging
logging.basicConfig(stream=sys.stderr)
sys.path.insert(0, "/var/www/catalog/")
from catalog import app as application
application.secret_key = 'supersecretkey'```
4. make another catalog ```sudo mkdir catalog``` (your route will look like this ~/var/www/catalog/catalog) ```cd catalog```
5. clone the application```sudo git clone https://github.com/longchass/Udacity-item-catalog-ofc-/tree/uda-linux-server```
6. configure the webserve```sudo nano /etc/apache2/sites-available/catalog.conf```
put this in
```<VirtualHost *:80>
    ServerName 54.252.223.127
    ServerAdmin admin@54.252.223.127
    WSGIDaemonProcess catalog python-path=/var/www/catalog:/var/www/catalog/venv/lib/python2.7/site-packages
    WSGIProcessGroup catalog
    WSGIScriptAlias / /var/www/catalog/catalog.wsgi
    <Directory /var/www/catalog/catalog/>
        Order allow,deny
        Allow from all
    </Directory>
    Alias /static /var/www/catalog/catalog/static
    <Directory /var/www/catalog/catalog/static/>
        Order allow,deny
        Allow from all
    </Directory>
    ErrorLog ${APACHE_LOG_DIR}/error.log
    LogLevel warn
    CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>```
7. ```run database_setup.py``` and ```database_init.py```



##Reference
thank you for the README (kongling893)[https://github.com/kongling893/Linux-Server-Configuration-UDACITY]

