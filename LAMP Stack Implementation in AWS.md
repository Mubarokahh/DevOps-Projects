
# P: LAMP Stack Implementation in AWS
Lamp stack is a software bundle used for web development.It comprises of different technologies that can be used to create a web solution. Lamp stack invoves setting up the folloeing technologies (Linux+Apache+MySQL+PHP/Perl/Python). In this project,I will configure a fully funcional development environment on linux.

## Step 1: Launching and Connecting to EC2 instance in AWS Cloud
I Spinned up an EC2 instance in AWS.I then open my mac terminal and ran the SSH comands in order to connect to the server I have running in AWS cloud.
![image](https://github.com/Mubarokahh/DevOps-Projects/assets/135038657/6a80e081-f961-4f33-b84b-d8bb501b0b24)

Below is the visual representation of how set up looks:

[image] https://user-images.githubusercontent.com/110112922/245434674-4f70859c-0d38-403b-bf27-ad1dde56a2e4.png!

[image](https://github.com/Mubarokahh/DevOps-Projects/assets/135038657/bc6a82b0-c2fb-47f3-a088-541e62fe552e)


## Step 2: Apache Installation & Firewall Update
The EC2 instance was configured to be able to serve the web server which in this context is the Apache.For the installation of Apache,the following actions were taken in the environment:
    
    1. Updating a list of packages in the package manager using 
    
    sudo apt update
    2. Run apache2 package installation: 
    
    sudo apt install apache2
    3. To verify that apache2 is running as a Service in my OS:
    
    sudo systemctl status apache2


![image](https://github.com/Mubarokahh/DevOps-Projects/assets/135038657/b585decf-5295-4f2e-97cc-1b5fed263de8)
The green highlighted and running light singifies that the web server has been launched in the cloud.Apache has been succefully installed.To access the web server in ubuntu, one of these command was used : curl http://localhost:80 or curl http://127.0.0.1:80
To test how THE Apache HTTP server can respond to requests from the Internet. I used a web browser of choice and try to access following using my public IP address.
![image](https://github.com/Mubarokahh/DevOps-Projects/assets/135038657/25a5e1ea-9cb9-495a-bdb4-4c887e8a121f)
This page indicates that the web server has been correctly installed andd accessible through my firewall.

## Step 3: Installing MySQL
After the web server has been installed and running, I installed a database management system to be able to store and manage data for the site in a relational database.
To acquire and install the software: `$ sudo apt install mysql-server `
NOTE: I am still in the linus enviroment connected to the EC2 server.
![image](https://github.com/Mubarokahh/DevOps-Projects/assets/135038657/a3a83e4b-7e6e-4ec7-923c-dd4c9a334ae7)
After the installation,I ran a security key that came pre-installed in MySQL, to remove some insecure default settings.and lockdown access to my database system.I congigured a password for the "Root" user, using mysql_native_password as default authentication method.`ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY 'PassWord.1';`

I started the interactive script by runnimg `sudo mysql_secure_installation` command. I accepted to set up  "Validate Password Plugin" when prompted.I sleected 2 for the password validation policy and then set up a password that suits the level og password validation policy. 
![image](https://github.com/Mubarokahh/DevOps-Projects/assets/135038657/7b018a5c-a1ef-4f5d-9f81-e5e69ebd7730)
After cofiguring MySQL, I tested if i am able to log in by running `sudo mysql -p`

## Step 4: Installing PHP
Alongside the PHP package, I also installed PHP-MySQL,a module that helps PHP to communicate with MySQL based databases.Lipache2-mod-PHP , a module that enables Apache to handle PHP files. To install these three packages at once . I ran the `sudo apt install php libapache2-mod-php php-mysql` command.
![image](https://github.com/Mubarokahh/DevOps-Projects/assets/135038657/d66a2a70-9d0a-4463-b625-c3d9d866e731)
The LAMP stack was.installed and operational

## Step 5: Creating a Virtual Host Using Apache

Creation of vitual host was done in these outlimed process:

      * Creation of the directory for projectlamp using mkdir command as follows:sudo mkdir /var/www/projectlamp
     
      * Assigning ownership of the directory with the current system user: sudo chown -R $USER:$USER /var/www/projectlamp
     
      * I created a new configuration file in Apache’s sites-available directory using your preferred command-line editor.I used Vi
     
      * This generated a blank file.I pasted in the balck file by hitting i to acess the imsert mode. Then I pasted:
      
![image](https://github.com/Mubarokahh/DevOps-Projects/assets/135038657/39a49d59-6b7f-4444-ad6f-16050616e3f4)
     
     * Save and quit by hitting esc key and typing :wq and pressing enter key
     
     * Enabling the new virtual host: $ sudo a2ensite projectlamp
     
     * Disabling the default website that comes installed with Apache: sudo a2dissite 000-default
     
     * Ensuring my configuration doesn’t contain any error: sudo apache2ctl configtest
     
     * Reloading Apache so these changes take effect:  sudo systemctl reload apache2
     
     * Created an index.html file in the location of my domain directory inorder to test that the virtual host works as expected : 
     
     ``sudo echo 'Hello Mubarokah,keep soaring  hostname' $(curl -s http://169.254.169.254/latest/meta-data/public-hostname) 'with public IP' $(curl -s http://169.254.169.254/latest/meta-data/public-ipv4) > `/var/www/projectlamp/i`ndex.htm
![image](https://github.com/Mubarokahh/DevOps-Projects/assets/135038657/6600e4b0-ce09-4db0-9800-a76d1e5c0b5b)
I refreshed the website to see of it worked..
![image](https://github.com/Mubarokahh/DevOps-Projects/assets/135038657/83bfd3f2-ea73-4bd0-9b79-7d064183b50d)
## Enabling PHP on Website
With the default DirectoryIndex settings on Apache, a file named index.html will always take precedence over an index.php file.

To change the default directoryindex on Apache, i ran this` sudo vim /etc/apache2/mods-enabled/dir.conf` command
![image](https://github.com/Mubarokahh/DevOps-Projects/assets/135038657/3d002197-64e7-4d28-b590-35c97c4f6ec4)
Then i ran the` $ sudo systemctl` reload apache2 to reload the website.
I created a PHP test script to confirm that Apache is able to handle and process requests for PHP files.

I generated a new file named index.php inside your custom web root folder: `vim /var/www/projectlamp/index.php`. This openned a blank page,and the following text which is valid PHP code inside the file:` <?php phpinfo();`
![image](https://github.com/Mubarokahh/DevOps-Projects/assets/135038657/2ff78233-7111-4f0e-b298-639ebcb3eeab)
At the end of this project, i removed the php file as it contains sensitive information about your PHP environment and my Ubuntu `server:sudo rm /var/www/projectlamp/index.php`






