
# WEB SOLUTION WITH WORDPRESS 

You are progressing in practicing to implement web solutions using different technologies. As a DevOps engineer you will most probably encounter PHP-based solutions since, even in 2021, it is the dominant web programming language used by more websites than any other programming language.

In this project you will be tasked to prepare storage infrastructure on two Linux servers and implement a basic web solution using WordPress. WordPress is a free and open-source content management system written in PHP and paired with MySQL or MariaDB as its backend Relational Database Management System (RDBMS).

This project includes two parts:

* Configure storage subsystem for Web and Database servers based on Linux OS. The focus of this part is to give you practical experience of working with disks, partitions and volumes in Linux.
* Install WordPress and connect it to a remote MySQL database server. This part of the project will solidify your skills of deploying Web and DB tiers of Web solution.

 ## Three-tier Architecture
Generally, web, or mobile solutions are implemented based on what is called the Three-tier Architecture.

Three-tier Architecture is a client-server software architecture pattern that comprise of 3 separate layers.

![image](https://github.com/Mubarokahh/DevOps-Projects/assets/135038657/99bc8422-5739-4518-926a-b5dbdfc43503)

Presentation Layer (PL): This is the user interface such as the client server or browser on your laptop.

Business Layer (BL): This is the backend program that implements business logic. Application or Webserver

Data Access or Management Layer (DAL): This is the layer for computer data storage and data access. Database Server or File System Server such as FTP server, or NFS Server

In this project, you will have the hands-on experience that showcases Three-tier Architecture while also ensuring that the disks used to store files on the Linux servers are adequately partitioned and managed through programs such as gdisk and LVM respectively.

## The 3-Tier Setup for this project

* A Laptop or PC to serve as a client

* An EC2 Linux Server as a web server (This is where you will install WordPress)

* An EC2 Linux server as a database (DB) server

  Across all project we have implemented previously, we used ‘Ubuntu’, but it is better to be well-versed with various Linux distributions, thus, for this projects we will use very popular distribution called ‘RedHat’ (it also has a fully compatible derivative – CentOS)

  * SIDENOTE: for Ubuntu server, when connecting to it via SSH/Putty or any other tool, we used ubuntu user, but for RedHat you will need to use ec2-user user. Connection string will look like ec2-user@<Public-IP>

  ## LAUNCH AN EC2 INSTANCE THAT WILL SERVE AS “WEB SERVER”

  Launch an EC2 instance that will serve as “Web Server”. Create 3 volumes in the same AZ as your Web Server EC2, each of 10 GiB

  ![image](https://github.com/Mubarokahh/DevOps-Projects/assets/135038657/768c420d-0530-41ef-9321-00b7ffaa90d8)




  ![image](https://github.com/Mubarokahh/DevOps-Projects/assets/135038657/551f9ce3-9a6c-4740-b18c-cedafaf64865)




  ![image](https://github.com/Mubarokahh/DevOps-Projects/assets/135038657/a583a9e4-8fb7-453b-851b-edc4d1402f2c)

  

  * Attach all three volumes one by one to your Web Server EC2 instance
 
  ![image](https://github.com/Mubarokahh/DevOps-Projects/assets/135038657/1e10a7b2-437d-4c15-a284-26f9cae7fc10)


 * Open up the Linux terminal to begin configuration

   
 ![image](https://github.com/Mubarokahh/DevOps-Projects/assets/135038657/e61dd763-6850-440e-aad7-d68702f60090)

 * Use lsblk command to inspect what block devices are attached to the server. Notice names of your newly created devices. All devices    in Linux reside in /dev/ directory. Inspect it with ls /dev/ and make sure you see all 3 newly created block devices there – their       names  will likely be xvdf, xvdh, xvdg.


![image](https://github.com/Mubarokahh/DevOps-Projects/assets/135038657/f1e53035-ba76-494e-a2fb-f3743afa86ae)

* Use df -h command to see all mounts and free space on your server

![image](https://github.com/Mubarokahh/DevOps-Projects/assets/135038657/c1d874da-5ad6-4812-952d-ea6c9dcc13a0)

* Use gdisk utility to create a single partition on each of the 3 disks

`sudo gdisk /dev/xvdf`

![image](https://github.com/Mubarokahh/DevOps-Projects/assets/135038657/7585b110-78c9-40c5-8c50-1c1331872cd7)

* Use lsblk utility to view the newly configured partition on each of the 3 disks.


![image](https://github.com/Mubarokahh/DevOps-Projects/assets/135038657/d289edd4-e4bb-4e96-8085-048ad214f854)

* Install lvm2 package using
  
 ` sudo yum install lvm2`.

 ![image](https://github.com/Mubarokahh/DevOps-Projects/assets/135038657/1a60266f-92fd-4c70-a8be-f3ebf6ec55e3)

 
 Run `sudo lvmdiskscan ` command to check for available partitions.

 ![image](https://github.com/Mubarokahh/DevOps-Projects/assets/135038657/e334d44c-4d26-4b35-8071-b5695db28f60)

 * Use pvcreate utility to mark each of 3 disks as physical volumes (PVs) to be used by LVM

   `sudo pvcreate /dev/xvdf1`
   
   
   `sudo pvcreate /dev/xvdg1`


   `sudo pvcreate /dev/xvdh1`

  ![image](https://github.com/Mubarokahh/DevOps-Projects/assets/135038657/5df7685a-e401-4de4-aaf9-ffb17e8861c7)

   * Verify that your Physical volume has been created successfully by running
   
     `sudo pvs`

  ![image](https://github.com/Mubarokahh/DevOps-Projects/assets/135038657/a37f2088-cef3-460d-8db3-00fe01247ff9)

   * Use vgcreate utility to add all 3 PVs to a volume group (VG). Name the VG webdata-vg
    
   `sudo vgcreate webdata-vg /dev/xvdh1 /dev/xvdg1 /dev/xvdf1`

   ![image](https://github.com/Mubarokahh/DevOps-Projects/assets/135038657/c4c9d210-36d3-452a-a806-cd8032df3305)
   

   * Verify that your VG has been created successfully by running

    `sudo vgs`
    

    ![image](https://github.com/Mubarokahh/DevOps-Projects/assets/135038657/35e678a3-33e3-44a7-bfa4-eaad885fcd13)


    * Use lvcreate utility to create 2 logical volumes. apps-lv (Use half of the PV size), and logs-lv Use the remaining space of the       PV size. NOTE: apps-lv will be used to store data for the Website while, logs-lv will be used to store data for logs.


    
      `sudo lvcreate -n apps-lv -L 14G webdata-vg`



      `sudo lvcreate -n logs-lv -L 14G webdata-vg`
      

     ![image](https://github.com/Mubarokahh/DevOps-Projects/assets/135038657/fb664a98-284e-4a62-b60f-ca489a1acd1a)
     


    * verify that your Logical Volume has been created successfully by running
    
       `sudo lvs`

     ![image](https://github.com/Mubarokahh/DevOps-Projects/assets/135038657/9d7297c8-afdb-4f11-b1ba-938130ab64d0)

   * Verify the entire setup
     
     `sudo vgdisplay -v #view complete setup - VG, PV, and LV`
     

     ![image](https://github.com/Mubarokahh/DevOps-Projects/assets/135038657/64db9097-ea38-41dc-83c2-9508650071b9)

     
     `sudo lsblk`

     ![image](https://github.com/Mubarokahh/DevOps-Projects/assets/135038657/dd647f13-b878-45d2-a2ba-a2738efff0fd)

     * Use mkfs.ext4 to format the logical volumes with ext4 filesystem
    
       `sudo mkfs -t ext4 /dev/webdata-vg/apps-lv`
       
  
       `sudo mkfs -t ext4 /dev/webdata-vg/logs-lv`

       ![image](https://github.com/Mubarokahh/DevOps-Projects/assets/135038657/1c17db33-77b9-46dc-b939-b491521f3a46)

      *  Create /var/www/html directory to store website files
       
       `sudo mkdir -p /var/www/html`
      
     *   Create /home/recovery/logs to store backup of log data
    
       `sudo mkdir -p /home/recovery/logs`
       
     *  Mount /var/www/html on apps-lv logical volume
    
       `sudo mount /dev/webdata-vg/apps-lv /var/www/html/`

       ![image](https://github.com/Mubarokahh/DevOps-Projects/assets/135038657/61db435a-4463-4817-b7aa-46cb5505edb8)


     * Use rsync utility to backup all the files in the log directory /var/log into /home/recovery/logs (This is required before               mounting the file system)
       
       `sudo rsync -av /var/log/. /home/recovery/logs/`

     * Mount /var/log on logs-lv logical volume. (Note that all the existing data on /var/log will be deleted. That is why step 15             above is very important)

      `sudo mount /dev/webdata-vg/logs-lv /var/log`

     * Restore log files back into /var/log directory

       `sudo rsync -av /home/recovery/logs/log/. /var/log`

       ![image](https://github.com/Mubarokahh/DevOps-Projects/assets/135038657/9610d800-bc3b-4b5d-95d8-a7982269e2cb)

     * Update /etc/fstab file so that the mount configuration will persist after restart of the server.The UUID of the device will be         used to update the /etc/fstab file;

       `sudo blkid`
       
       `sudo vi /etc/fstab`

       ![image](https://github.com/Mubarokahh/DevOps-Projects/assets/135038657/f2264645-49e3-40c6-ae0d-069199f7b218)
       

    * Update /etc/fstab in this format using your own UUID and rememeber to remove the leading and ending quotes.


`      ![image](https://github.com/Mubarokahh/DevOps-Projects/assets/135038657/dee7071d-5748-4530-bf3c-298909a7882a)
     
`
    * Test the configuration and reload the daemon

        `sudo mount -a`
        
        `sudo systemctl daemon-reload`

        

   *  Verify the set up ny running df -h , output must look like this

![image](https://github.com/Mubarokahh/DevOps-Projects/assets/135038657/d430d521-5673-40e5-b3d9-7344dd8a7eae)

    

    

     

   ## PREPARE THE DATABASE SERVER

   
   * Launch a second RedHat EC2 instance that will have a role – ‘DB Server’
Repeat the same steps as for the Web Server, but instead of apps-lv create db-lv and mount it to /db directory instead of /var/www/html/.

![image](https://github.com/Mubarokahh/DevOps-Projects/assets/135038657/365dbef6-d9ee-472a-92ae-a75e80d005a1)

![image](https://github.com/Mubarokahh/DevOps-Projects/assets/135038657/33c378cb-5956-4c33-9449-5e2bcb91f2f7)

![image](https://github.com/Mubarokahh/DevOps-Projects/assets/135038657/6f3e03b8-ae01-4ffd-9e69-ed4e93a08107)

![image](https://github.com/Mubarokahh/DevOps-Projects/assets/135038657/68efdbcb-5b41-4405-aeb3-f58945b8daa8)

![image](https://github.com/Mubarokahh/DevOps-Projects/assets/135038657/c28318ed-496a-4def-ab99-c4082a8e3087)

![image](https://github.com/Mubarokahh/DevOps-Projects/assets/135038657/06dc7885-93fd-4fb9-8261-66dda53752dd)

![image](https://github.com/Mubarokahh/DevOps-Projects/assets/135038657/f18f33ad-736c-455c-a119-fe2555f40df9)

![image](https://github.com/Mubarokahh/DevOps-Projects/assets/135038657/dd9b911c-1bb4-4f6a-b40d-79545a32bcac)

![image](https://github.com/Mubarokahh/DevOps-Projects/assets/135038657/6a3d89e8-8f71-452d-96c7-105d4629e106)

##  Install WordPress on your Web Server EC2

* Update the repository
  
  `sudo yum -y update`

* Install wget, Apache and it’s dependencies
  
  `sudo yum -y install wget httpd php php-mysqlnd php-fpm php-json`

* Start Apache
  
  `sudo systemctl enable httpd`
  
  `sudo systemctl start httpd`

* To install PHP and it’s depemdencies
  
`sudo yum install https://dl.fedoraproject.org/pub/epel/epel-release-latest-8.noarch.rpm
 sudo yum install yum-utils http://rpms.remirepo.net/enterprise/remi-release-8.rpm
 sudo yum module list php
 sudo yum module reset php
 sudo yum module enable php:remi-7.4
 sudo yum install php php-opcache php-gd php-curl php-mysqlnd
 sudo systemctl start php-fpm
 sudo systemctl enable php-fpm
 setsebool -P httpd_execmem 1`

 * Restart Apache
   
  `sudo systemctl restart httpd`

 * Download wordpress and copy wordpress to var/www/html
   
  `mkdir wordpress
  cd   wordpress
  sudo wget http://wordpress.org/latest.tar.gz
  sudo tar xzvf latest.tar.gz
  sudo rm -rf latest.tar.gz
  cp wordpress/wp-config-sample.php wordpress/wp-config.php
  cp -R wordpress /var/www/html/`

 * Configure SELinux Policies
   
  `sudo chown -R apache:apache /var/www/html/wordpress
  sudo chcon -t httpd_sys_rw_content_t /var/www/html/wordpress -R
  sudo setsebool -P httpd_can_network_connect=1`











  




  




