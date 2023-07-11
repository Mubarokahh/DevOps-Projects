# Project 2: LEMP STACK Implementation
LEMP STACK is a bundle of technologies used for web development. The combination of these technologies create web solutions

## STEP 1:Launching and connecting to EC2 environment in AWS

* I launched an EC2 instance through AWS.I then launched the SSH commands on my Mac terminal and connected to the server I was operating in the AWS cloud.

![image](https://github.com/Mubarokahh/DevOps-Projects/assets/135038657/64b118b6-1ee5-4fe1-84d5-1c5f3f0b48e8)

## STEP 2:Instaliing The Nginx Web Server
For this project,Nginx will be used to show web pages to the site's visitors.Apt package manager was used to install this package. 

* The following commands will be run to install Nginx.
* (1) `sudo apt update`
*  (2)`sudo apt install nginx`
  
![image](https://github.com/Mubarokahh/DevOps-Projects/assets/135038657/425b948f-a198-442d-8b8a-68bdf465d603)

* To verify if Nginx is running sucessfully in the virtual machine. The following syntax was run: Sudo systemtcl status Nginx

  ![image](https://github.com/Mubarokahh/DevOps-Projects/assets/135038657/3ba291d1-48d7-45a8-88ee-696e5f91f63d)

The server running in EC2 was configured to receive traffic by the web server.Thus, the EC2 was configured to open inbound connection through Port 80

![image](https://github.com/Mubarokahh/DevOps-Projects/assets/135038657/b9719b58-c2fd-4a26-af20-2740dc6ca0f8)

* To access the server loacally from my mac termical: `curl http://localhost:80`

![image](https://github.com/Mubarokahh/DevOps-Projects/assets/135038657/1db00187-44bf-4df4-9dc7-0aab5df79589)

* To check if the high performance web server (Nginx) can reponse to request on the internet.I insert my public IP address into a web brower: 'http://<Public-IP-Address>:80'

  ![image](https://github.com/Mubarokahh/DevOps-Projects/assets/135038657/894214b7-df31-41fe-9b10-1e0d519e2db4)

The image display above shows that Nginx server is correectly installed.

## STEP 3:Installation of MySQL

After the web server has been installed and running, I installed a database management system to be able to store and manage data for the site in a relational database. 

* To acquire and install the software: `sudo apt install mysql-server`

![image](https://github.com/Mubarokahh/DevOps-Projects/assets/135038657/010b04ed-8fef-4266-b6d6-24d210785169)

* Upon the completion of MySQL server, log into the SQL console: `sudo Mysql`
*  This will connect to the MySQL server as the administrative database user root, which is inferred by the use of sudo when running this command.

  ![image](https://github.com/Mubarokahh/DevOps-Projects/assets/135038657/c03ec69b-3966-4c19-882e-84beef62d679)

* After MySQL has been sucessfully installed,I configured a password for the "Root" user using mysql_native_password as default authentication method. `ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY 'PassWord.1`

  ![image](https://github.com/Mubarokahh/DevOps-Projects/assets/135038657/b9d9eef5-84fc-4fb1-ab6d-eecaecee684f)

 * it is require to run the security scipt that comes pre installed with MySQL server.This removes insecure default setting and lock acess to my database system.I started the interactive scrip by running `sudo mysql_secure_installation`
 * I accepted to set up "Validate Password Plugin" when prompted.I sleected 2 for the password validation policy and then set up a password that suits the level of password validation policy.
 * To test if I am able to log in into mysql console: `sudo mysql -p`
   
## STEP 4:Installing PHP

PHP is used to process code and generate dynamic content for the wen server.PHP-MySQL, a PHP module that helps PHP to communicate with mySQL will be installed.
* SIDENOTE: Unlike apache that embeds PHP interpreter in each requets, Nginx requires an external program to handle PHP processing and act as a bridge between the PHP interpreter itself and the web server.Thus, requiring additional configuration.PHP-FPM (“PHP fastCGI process manager”) will be installed for this purpose.
* PHP-FPM will be installed along side PHP-MYSQL (PHP module that allows PHP to communicate with MySQL-based databases)
* Installing the both at once:  `sudo apt install php-fpm php-mysql`

  ![image](https://github.com/Mubarokahh/DevOps-Projects/assets/135038657/58293c24-5b30-4df4-aeee-c58c628c1295)


## STEP 5:Configuring Nginx To Use PHP Processor

On Ubuntu 20.04, Nginx has one server block enabled by default and is configured to serve documents out of a directory at /var/www/html. The following steps are taken to setup a web directory called projectLEMP and configure Nginx to use PHP processor.
* Creating a root web directory for the domain: `sudo mkdir /var/www/projectLEMP`

  ![image](https://github.com/Mubarokahh/DevOps-Projects/assets/135038657/df61fa10-2b9e-41a7-acc4-4ee905d79d90)

* Assigning ownership of the directory with the $USER environment variable, which will reference your current system user: s`udo chown -R $USER:$USER /var/www/projectLEMP`
* opening a new configuration file in Nginx’s sites-available directory called projectLEMP: 'sudo nano /etc/nginx/sites-available/projectLEMP'
* The following script will be added in the created file
'#/etc/nginx/sites-available/projectLEMP
`
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
    `
* Activating the configuration by linking to the config file from Nginx’s sites-enabled directory: `sudo ln -s /etc/nginx/sites-available/projectLEMP /etc/nginx/sites-enabled/`

  ![image](https://github.com/Mubarokahh/DevOps-Projects/assets/135038657/b28294bd-8c20-4dc1-aaa1-ba1904267da3)

* Test configuration for syntax by typing:` sudo nginx -t`
  
  ![image](https://github.com/Mubarokahh/DevOps-Projects/assets/135038657/33d7e5bd-81a8-4348-a5ec-76bed92407bb)

* Disabling default Nginx host that is currently configured to listen on port 80: 'sudo unlink /etc/nginx/sites-enabled/default'
* Reload Nginx to apply changes: `sudo systemctl reload nginx`
* Create an index.html file in that location so that we can test that your new server block works as expected:`sudo echo `Hello LEMP from hostname' $(curl -s http://169.254.169.254/latest/meta-data/public-hostname)` 'with public IP' `$(curl -s http://169.254.169.254/latest/meta-data/public-ipv4) > /var/www/projectLEMP/index.html``
Testing the website URL using my IP address: http://<Public-IP-Address>:80

![image](https://github.com/Mubarokahh/DevOps-Projects/assets/135038657/d23e8544-f91e-4e39-96f0-116382a84792)


## STEP 6:Testing PHP with Nginx
Test LEMP stack to  validate that Nginx can correctly hand .php files off to your PHP processor.
* It can be done by creating a test PHP file in your document root. Open a new file called info.php within your document root in your text editor:`sudo nano /var/www/projectLEMP/info.php`
* Paste the following script to the file: 
`<?php
phpinfo();`
* The page can be acess in the web browser through`: http://Public_IP_address/info.php`
  
  ![image](https://github.com/Mubarokahh/DevOps-Projects/assets/135038657/269ec5d0-5eb3-4f50-aa23-7929ba9f5420)

  ## STEP 7: Retrieving data from MySQL database with PHP
  A test database (DB) with simple “To do list” and configure access to it will be craeted, so the Nginx website would be able to query data from the DB and display it.
Creating a database named example_database and a user named example_user, but these names can be replaced with different values
* Connecting to MySQL console:  `sudo mysql`
* Creating a new database: `mysql> CREATE DATABASE example_database;`
* Creating a new user and granting him full privileges on the database using mysql_native_password as default authentication method: `mysql>  CREATE USER 'example_user'@'%' IDENTIFIED WITH mysql_native_password BY 'password'; mysql> GRANT ALL ON example_database.* TO 'example_user'@'%';`
* Exit MySQL console: mysql> exit
* Testing if the new user has the proper permissions by logging in to the MySQL console again using the custom user credentials: `$ mysql -u example_user -p`
* To check the database created: `mysql> SHOW DATABASES;`

![image](https://github.com/Mubarokahh/DevOps-Projects/assets/135038657/69f816cb-fdc6-4314-b7a8-725c411adfb1)

* Creating a test table named todo_list: 
`CREATE TABLE example_database.todo_list (item_id INT AUTO_INCREMENT,content VARCHAR(255),PRIMARY KEY(item_id));`
* Insert a few rows of content in the test table: `mysql> INSERT INTO example_database.todo_list (content) VALUES ("My first important item");`
* To confirm that the data was successfully created:` mysql>  SELECT * FROM example_database.todo_list;`
* Exit the MySQL:` mysql> exit`

   Creating a PHP script that will connect to MySQL and query for your content.
  * Create a new PHP file in your custom web root directory using your preferred editor: `nano /var/www/projectLEMP/todo_list.php`
  * The folloeing content will be added into the todo_list.php script:
   ` <?php
$user = "example_user";
$password = "password";
$database = "example_database";
$table = "todo_list";

try {
  $db = new PDO("mysql:host=localhost;dbname=$database", $user, $password);
  echo "<h2>TODO</h2><ol>";
  foreach($db->query("SELECT content FROM $table") as $row) {
    echo "<li>" . $row['content'] . "</li>";
  }
  echo "</ol>";
} catch (PDOException $e) {
    print "Error!: " . $e->getMessage() . "<br/>";
    die();
}`
* To access this page in any web browser, I used my public IP address followed by /todo_list.php `http://<Public_domain_or_IP>/todo_list.php`
 ![image](https://github.com/Mubarokahh/DevOps-Projects/assets/135038657/83aad96c-f1de-4ff1-9b15-deabe8666551)



