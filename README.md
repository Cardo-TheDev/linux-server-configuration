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
 
 
 ### 3rd Part Resourses Utilised
 - Leon's Blog [Apache2, Python3 and pip3](http://leonwang.me/post/deploy-flask)
 - Digital Ocean [Postgresql and psycopg2](https://www.digitalocean.com/community/tutorials/how-to-install-and-use-postgresql-on-ubuntu-18-04)
 - Udacity [Amazon Lightsail and other helpful resources](https://Udacity.com)
