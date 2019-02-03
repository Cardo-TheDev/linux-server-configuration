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
 
 `sudo nano /etc/ssh/sshd_config` and change 22 to 2200. `sudo service 
 
 - Configure the ufw
 
 `sudo ufw default deny incoming` to block all incoming requests. `sudo ufw default allow outgoing` to allow all outgoing requests by server. `sudo ufw allow 2200/tcp` to allow all TCP connections through Port 2200 for SSH. `sudo ufw allow www` to support _HTTP_. `sudo ufw allow 123/tcp` to allow NTP. Enable firewall with `sudo ufw enable`. Check status with `sudo ufw status` to verify which ports are active.
 
 - Check for updates and upgrade package source list
 
 `sudo apt-get update` `sudo apt-get upgrade`
 
 - Create user **_grader_** and give sudo access
 
 `sudo adduser grader` to create a new user named _grader_. `sudo nano /etc/sudoers.d/grader` and input the text: _#CLOUD_IMG: This file was created/modified by the Cloud Image build process
grader ALL=(ALL) NOPASSWD:ALL_ to add _grader_ to list of _sudoers_. 
 
 - Secure server by only allowing key based authentication 
 
 Download the default _SSH key pair_ that accompanies the ubuntu instance from Amazon Lightsail account. Copy the contents of _/home/ubuntu/.ssh/authorized_keys. Then, `sudo nano /home/grader/.ssh/authorized_keys` and paste the content. User _grader_ is now able to log in with key: ssh -i name-of-key.pem grader@54.93.243.84 -p 2200. Dis


 
 - Enable  _grader_ to login with only key based authentication
 
 Create a d
 
 
 ### 3rd Part Resourses Utilised
 - Leon's Blog [Apache2, Python3 and pip3](http://leonwang.me/post/deploy-flask)
 - Digital Ocean [Postgresql and psycopg2](https://www.digitalocean.com/community/tutorials/how-to-install-and-use-postgresql-on-ubuntu-18-04)
 - Udacity [Amazon Lightsail and other helpful resources](https://Udacity.com)
