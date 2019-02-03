# Linux-Server-Configuration

## About
I took a a baseline installation of a Linux server, using [Amazon Lightsail](https://lightsail.aws.amazon.com/) and prepared it to host my web applications. I made sure to install updates, secure my server from a number of attack vectors, configure the uncomplicated firewall (ufw), install and configure a database server (postgresql), and deploy my web applications onto it.

- IP Address: 54.93.243.84
- SSH Port: 2200
- URL: http://54.93.243.84.xip.io/

### Softwares Installed
`sudo apt-get install python3-pip apache2 libapache2-mod-wsgi-py3 git postgresql`
 
`pip3 install flask oauth2client requests httplib2 psycopg2`
 
### Configurations Made
- Start a new Ubuntu Linux server instance on [Amazon Lightsail](https://lightsail.aws.amazon.com/).
 
- Change the SSH port from 22 to 2200
 
`sudo nano /etc/ssh/sshd_config` and change 22 to 2200. `sudo service ssh restart` for changes to take effect.
 
- Configure the ufw
 
`sudo ufw default deny incoming` to block all incoming requests. `sudo ufw default allow outgoing` to allow all outgoing requests by server. `sudo ufw allow 2200/tcp` to allow all TCP connections through Port 2200 for SSH. `sudo ufw allow www` to support _HTTP_. `sudo ufw allow 123/tcp` to allow NTP. Enable firewall with `sudo ufw enable`. Check status with `sudo ufw status` to verify which ports are active.
 
- Check for updates and upgrade package source list
 
`sudo apt-get update` `sudo apt-get upgrade`
 
- Create user **_grader_** and give sudo access
 
`sudo adduser grader` to create a new user named _grader_. `sudo nano /etc/sudoers.d/grader` and input the text: _#CLOUD_IMG: This file was created/modified by the Cloud Image build process
 
grader ALL=(ALL) NOPASSWD:ALL_ to add _grader_ to list of _sudoers_. 
 
- Secure server by only allowing key based authentication 
 
Download the default _SSH key pair_ that accompanies the ubuntu instance from Amazon Lightsail account. While logged in as _grader_, copy the contents of _/home/ubuntu/.ssh/authorized_keys. Create a directory, `mkdir .ssh`. Then, `nano .ssh/authorized_keys` and paste the copied content. User _grader_ is now able to log in with key: ssh -i name-of-key.pem grader@54.93.243.84 -p 2200. Set file permissions for _.ssh/_ and _authorized_keys_ that prevents other users/groups from gaining access: 'chmod 700 .ssh' and 'chmod 644 .ssh/authorized_keys'. Finally, disable password login by editing _/etc/ssh/sshd_config_. Change the 'yes' line on 'PasswordAuthentication' to 'no'.
 
- Configure local timezon to UTC
 
`sudo dpkg-reconfigure tzdata`
 
- Configure Apache to serve a Python mod_wsgi application
 
First, 
 
`cd /var/www/`
  
`sudo mkdir catalog`
  
`sudo git clone https://github.com/Cardo-TheDev/catalog-project catalog`. If main application file name is not _\_\_init\_\_.py_, do so; this makes the WSGI recognize it.
  
`sudo nano catalog.wsgi`. Add the following configuration below:
```
#!/usr/bin/python
import sys
import logging
logging.basicConfig(stream=sys.stderr)
sys.path.insert(0,"/var/www/catalog/")

from catalog import app as application
application.secret_key = 'super_secret_key'
```
  
`sudo nano /et/apache2/sites-available/catalog.conf` and configure with settings below:
```
<VirtualHost *:80>
           ServerName 54.93.243.84
	          ServerAlias 54.93.243.84.xip.io
           ServerAdmin admin@54.93.243.84
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
</VirtualHost>
```
- Configure Postgresql

`sudo nano /etc/postgresql/9.5/main/pg_hba.conf` and make sure remote connections to database is not allowed.

   
 ### 3rd Part Resourses Utilised
 - Leon's Blog [Apache2, Python3 and pip3](http://leonwang.me/post/deploy-flask)
 - Digital Ocean [Postgresql and psycopg2](https://www.digitalocean.com/community/tutorials/how-to-install-and-use-postgresql-on-ubuntu-18-04)
 - Udacity [Amazon Lightsail and other helpful resources](https://Udacity.com)
