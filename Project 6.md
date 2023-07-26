
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




  




