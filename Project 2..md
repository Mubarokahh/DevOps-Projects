# Project 2: LEMP STACK Implementation
LEMP STACK is a bundle of technologies used for web development. The combination of these technologies create web solutions

## STEP 1:Launching and connecting to EC2 environment in AWS

* I launched an EC2 instance through AWS.I then launched the SSH commands on my Mac terminal and connected to the server I was operating in the AWS cloud.

! (image.png)

## STEP 2:Instaliing The Nginx Web Server

For this project,Nginx will be used to show web pages to the site's visitors.Apt package manager was used to install this package. 
* The following commands will be run to install Nginx.(1) 'sudo apt update' (2) 'sudo apt install nginx' . 
* To verify if Nginx is running sucessfully in the virtual machine. The following syntax was run:'Sudo systemtcl status Nginx'
image.png 
The server running in EC2 was configured to receive traffic by the web server.Thus, the EC2 was configured to open inbound connection through Port 80
image.png

* To acess the server loacally from my mac termical: 'curl http://localhost:80'
image.png
* To check if the high performance web server (Nginx) can reponse to request on the internet.I insert my public IP address into a web brower: 'http://<Public-IP-Address>:80'

The image display above shows that Nginx server is correectly installed.

## STEP 3:Installation of MySQL

After the web server has been installed and running, I installed a database management system to be able to store and manage data for the site in a relational database. 
* To acquire and install the software:'$ sudo apt install mysql-server' 
* Upon the completion of MySQL server, log into the SQL console by typing 'Sudo Mysql' This will connect to the MySQL server as the administrative database user root, which is inferred by the use of sudo when running this command.
* After MySQL has been sucessfully installed,I configured a password for the "Root" user using mysql_native_password as default authentication method. 'ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY 'PassWord.1'
 * it is require to run the security scipt that comes pre installed with MySQL server.This removes insecure default setting and lock acess to my database system.I started the interactive scrip by running 'sudo mysql_secure_installation'
 * I accepted to set up "Validate Password Plugin" when prompted.I sleected 2 for the password validation policy and then set up a password that suits the level of password validation policy.
 * To test if I am able to log in into mysql console: 'sudo mysql -p'

## STEP 4:Installing PHP

PHP is used to process code and generate dynamic content for the wen server.PHP-MySQL, a PHP module that helps PHP to communicate with mySQL will be installed.
* SIDENOTE: Unlike apache that embeds PHP interpreter in each requets, Nginx requires an external program to handle PHP processing and act as a bridge between the PHP interpreter itself and the web server.Thus, requiring additional configuration.PHP-FPM (“PHP fastCGI process manager”) will be installed for this purpose.
* PHP-FPM will be installed along side PHP-MYSQL (PHP module that allows PHP to communicate with MySQL-based databases)

## STEP 5:Configuring Nginx To Use PHP Processor

On Ubuntu 20.04, Nginx has one server block enabled by default and is configured to serve documents out of a directory at /var/www/html. The following steps are taken to setup a web directory called projectLEMP and configure Nginx to use PHP processor.
* Creating a root web directory for the domain: 'sudo mkdir /var/www/projectLEMP'
* Assigning ownership of the directory with the $USER environment variable, which will reference your current system user: 'sudo chown -R $USER:$USER /var/www/projectLEMP'
* opening a new configuration file in Nginx’s sites-available directory called projectLEMP: 'sudo nano /etc/nginx/sites-available/projectLEMP'
* The following script will be added in the created file
'#/etc/nginx/sites-available/projectLEMP

server {
    listen 80;
    server_name projectLEMP www.projectLEMP;
    root /var/www/projectLEMP;

    index index.html index.htm index.php;

    location / {
        try_files $uri $uri/ =404;
    }

    location ~ \.php$ {
        include snippets/fastcgi-php.conf;
        fastcgi_pass unix:/var/run/php/php7.4-fpm.sock;
     }

    location ~ /\.ht {
        deny all;
    }
    
* Activating the configuration by linking to the config file from Nginx’s sites-enabled directory: 'sudo ln -s /etc/nginx/sites-available/projectLEMP /etc/nginx/sites-enabled/'
* Teat configuration for syntax by typing: 'sudo nginx -t'
* Disabling default Nginx host that is currently configured to listen on port 80: 'sudo unlink /etc/nginx/sites-enabled/default'
* Reload Nginx to apply changes: 'sudo systemctl reload nginx'
* Create an index.html file in that location so that we can test that your new server block works as expected:'sudo echo 'Hello LEMP from hostname' $(curl -s http://169.254.169.254/latest/meta-data/public-hostname) 'with public IP' $(curl -s http://169.254.169.254/latest/meta-data/public-ipv4) > /var/www/projectLEMP/index.html'
Testing the website URL using my IP address: http://<Public-IP-Address>:80

## STEP 6:Testing PHP with Nginx
Test LEMP stack to  validate that Nginx can correctly hand .php files off to your PHP processor.
* It can be done by creating a test PHP file in your document root. Open a new file called info.php within your document root in your text editor: 'sudo nano /var/www/projectLEMP/info.php'
* Paste the following script to the file: 
'<?php
phpinfo();'
* The page can be acess in the web browser through: 'http://Public_IP_address/info.php'
  
  ![image](https://github.com/Mubarokahh/DevOps-Projects/assets/135038657/269ec5d0-5eb3-4f50-aa23-7929ba9f5420)

  ## Retrieving data from MySQL database with PHP
  A test database (DB) with simple “To do list” and configure access to it will be craeted, so the Nginx website would be able to query data from the DB and display it.
Creating a database named example_database and a user named example_user, but these names can be replaced with different values
* Connecting to MySQL console:  `sudo mysql`
* Creating a new database: `mysql> CREATE DATABASE example_database;`
* Creating a new user and granting him full privileges on the database using mysql_native_password as default authentication method: `mysql>  CREATE USER 'example_user'@'%' IDENTIFIED WITH mysql_native_password BY 'password'; mysql> GRANT ALL ON example_database.* TO 'example_user'@'%';`
* Exit MySQL console: mysql> exit
* Testing if the new user has the proper permissions by logging in to the MySQL console again using the custom user credentials: `$ mysql -u example_user -p`
* To check the database created: `mysql> SHOW DATABASES;`
*

