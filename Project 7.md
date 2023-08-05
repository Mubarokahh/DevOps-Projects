
 # DEVOPS TOOLING WEBSITE SOLUTION

  In this project, a set of Devops tools will be introduced to help the team in a day to day in managing,developing,testing, deploying and monitoring diffferent projects.
  The tools that will be used are well known tools and widely used by Devops teams, a single Devops toolind solution that will consist of the following:

*  Jenkins – free and open source automation server used to build CI/CD pipelines.
  
*  Kubernetes – an open-source container-orchestration system for automating computer application deployment, scaling, and management.

*  Jfrog Artifactory – Universal Repository Manager supporting all major packaging formats, build tools and CI servers. Artifactory.

*  Rancher – an open source software platform that enables organizations to run and manage Docker and Kubernetes in production.
Grafana – a multi-platform open source analytics and interactive visualization web application.

*  Prometheus – An open-source monitoring system with a dimensional data model, flexible query language, efficient time series database and modern alerting approach.

*  Kibana – Kibana is a free and open user interface that lets you visualize your Elasticsearch data and navigate the Elastic Stack.

  ## Setup and technologies used in this Project

  The aim of this project is implementation of a tooling website solution which gives acess to Devops tools within the corporate infracturucture easily accessible.
  A solution that consists of the folowing will be implemented:

 *  Infrastructure: AWS
  
 *  Webserver Linux: Red Hat Enterprise Linux 8

 *  Database Server: Ubuntu 20.04 + MySQL

 *  Storage Server: Red Hat Enterprise Linux 8 + NFS Server

 *  Programming Language: PHP

 *  Code Repository: GitHub

  ## Setting up the NFS server

  I spinned up an EC2 instance with RHEL Linux 8 Operating System  that will be used as the NFS server, Once the server started running, i created 3 volumes and attached them to the NFS server.

   ![image](https://github.com/Mubarokahh/DevOps-Projects/assets/135038657/2d343f54-58af-42ce-bc97-684dd04b0ee0)

  ##  Creating And Mounting Logical Volumes On The EC2 Instance For The NFS Server

  Instead of formating the disks as ext4 you will have to format them as xfs.

  * To inspect the blocks that are connected to the server, run: `lsblk`

  * To check the free space on the srver: `df -h`

  * Use gdisk utility to create  a single partition on each disk

    ` sudo gdisk /dev/xvdf`

    ` sudo gdisk /dev/xvdg`
 
    ` sudo gdisk /dev/xvdh`

     NOTE: For each partition, the following steps are required:

    Entering the key n to create new partition and selecting the default settings by hitting enter

    The partition type I selected is linux file system by entering its HEX code 8300

    Entering the p key to confirm the partition is created

    Entering the w key to write the partition and confirming with y key

    * To confirm that each of the disks has been sucessfully partitioned ; `lsblk`

      ![image](https://github.com/Mubarokahh/DevOps-Projects/assets/135038657/797770ba-896d-46aa-89bd-40b876b680ee)


      ![image](https://github.com/Mubarokahh/DevOps-Projects/assets/135038657/f06589b2-cfb4-4b33-a3f1-ba5d64a20347)


      ![image](https://github.com/Mubarokahh/DevOps-Projects/assets/135038657/b6abe421-dc06-44e3-871a-b5399688d634)

      ![image](https://github.com/Mubarokahh/DevOps-Projects/assets/135038657/1734055e-d384-4b7b-8a8b-3b61f7ddc95e)

    * Install lvm2 package
   
      `sudo yum install lvm2`

       ![image](https://github.com/Mubarokahh/DevOps-Projects/assets/135038657/89a69c4d-0831-4cb3-b87b-f3d174525d3e)

    * To check for available partitions, run
   
      `sudo lvmdiskscan `

       ![image](https://github.com/Mubarokahh/DevOps-Projects/assets/135038657/1d735ad2-e426-4495-b8e7-e4cc0c3a4d52)
      

    * Use pvcreate utility to mark each of 3 disks as physical volumes (PVs) to be used by LVM

       `sudo pvcreate /dev/xvdf1`

       `sudo pvcreate /dev/xvdg1`

       `sudo pvcreate /dev/xvdh1`


       ![image](https://github.com/Mubarokahh/DevOps-Projects/assets/135038657/555db8ac-cc26-4d86-bb2f-b8ff1950505e)
      

     *  Verify that your Physical volume has been created successfully by running

         ` sudo pvs`

     *  Use vgcreate utility to add all 3 PVs to a volume group (VG). Name the VG filedata-vg
   
       `sudo vgcreate filedata-vg  /dev/xvdf1 /dev/xvdg1 /dev/xvdh1`

       ![image](https://github.com/Mubarokahh/DevOps-Projects/assets/135038657/6bada589-fca6-4060-9243-c4bbb5f161e3)

    * Verify that your VG has been created successfully by running
   
       `sudo vgs`

    * Use lvcreate utility to create 3 logical volumes,lv-opt lv-apps, and lv-logs
   
       `sudo lvcreate –n lv-opt –L 9G filedata-vg`
      
       `sudo lvcreate –n lv-apps –L 9G filedata-vg`
      
       `sudo lvcreate –n lv-logs –L 9G filedata-vg`


       ![image](https://github.com/Mubarokahh/DevOps-Projects/assets/135038657/d5f3cb6e-d4a9-4b5a-b491-cf69b45cd3b1)

      * Verify the whole set up
     
       ` sudo vgdisplay -v`

      ![image](https://github.com/Mubarokahh/DevOps-Projects/assets/135038657/6397f46b-2bf2-4951-a03f-a8617ff23f09)
   
      * Format the logical volumes created with xfs filesystem:

          `sudo mkfs –t xfs /dev/filedata-vg/lv-opt`
        
          `sudo mkfs –t xfs /dev/filedata-vg/lv-apps`
        
          `sudo mkfs –t xfs /dev/filedata-vg/lv-log`s


        ![image](https://github.com/Mubarokahh/DevOps-Projects/assets/135038657/719ea7ca-25fa-4b99-afa1-c9aa15f504d2)
   
      * Creating a directory where the 3 logical volumes will be mounted in /mnt directory:
        `
         `sudo mkdir /mnt/opt
        
         `sudo mkdir /mnt/apps`
        
         `sudo mkdir /mnt/logs`

      * Create mount points on /mnt directory for the logical volumes as follow:

         Mount lv-apps on /mnt/apps – To be used by webservers

         Mount lv-logs on /mnt/logs – To be used by webserver logs

         Mount lv-opt on /mnt/opt – To be used by Jenkins server in Project 8
   
          ![image](https://github.com/Mubarokahh/DevOps-Projects/assets/135038657/3bae1ce3-b438-4c80-b3d0-f792af22d0a3)


      *  Install NFS server, configure it to start on reboot and make sure it is u and running
        
         `sudo yum -y update`
         
         `sudo yum install nfs-utils -y`
         
         `sudo systemctl start nfs-server.service`
         
         `sudo systemctl enable nfs-server.service`
         
         `sudo systemctl status nfs-server.service`

          ![image](https://github.com/Mubarokahh/DevOps-Projects/assets/135038657/31add7a7-67ea-44b7-bc43-a9e8fafd1640)

          ![image](https://github.com/Mubarokahh/DevOps-Projects/assets/135038657/415e7d18-f3a8-402b-b5c5-f8c68be946e7)

     * Export the mounts for webservers’ subnet cidr to connect as clients. For simplicity, you will install your all three Web
          Servers inside the same subnet, but in production set up you would probably want to separate each tier inside its own subnet            for higher level of security.
            Note:To check your subnet cidr – open your EC2 details in AWS web console and locate ‘Networking’ tab and open a Subnet link

        ![image](https://github.com/Mubarokahh/DevOps-Projects/assets/135038657/a76fcc5a-b67a-45f7-a7ee-faf21c02bdd8)

     * Make sure we set up permission that will allow our Web servers to read, write and execute files on NFS:

             `sudo chown -R nobody: /mnt/apps`
             
             `sudo chown -R nobody: /mnt/logs`

             `sudo chown -R nobody: /mnt/opt`

            ` sudo chmod -R 777 /mnt/apps`

             `sudo chmod -R 777 /mnt/logs`

             `sudo chmod -R 777 /mnt/opt`
         


        ![image](https://github.com/Mubarokahh/DevOps-Projects/assets/135038657/e0c2162a-2927-49db-a6f1-07026a7e8f8c)

     * Restarting the nfs service

            `sudo systemctl restart nfs-server.service`


     * Configure access to NFS for clients within the same subnet (example of Subnet CIDR - 172.31.80.0/20


        -  Open the NFS port file
                
          `sudo vi /etc/exports`
       
              -  Paste the following

                	/mnt/apps 172.31.80.0/20(rw,sync,no_all_squash,no_root_squash)
                	/mnt/logs 172.31.80.0/20(rw,sync,no_all_squash,no_root_squash)
                	/mnt/opt 172.31.80.0/20(rw,sync,no_all_squash,no_root_squash)

       ![image](https://github.com/Mubarokahh/DevOps-Projects/assets/135038657/2da5e542-3f1b-43c3-b745-48cf1d157b09)

                ` sudo exportfs -arv`

       ![image](https://github.com/Mubarokahh/DevOps-Projects/assets/135038657/48c20657-7a8a-4d82-83f7-4c161d86ef7a)

      * Check which port is used by NFS and open it using Security Groups (add new Inbound Rule)
           
            `rpcinfo -p | grep nfs`    

     ![image](https://github.com/Mubarokahh/DevOps-Projects/assets/135038657/13e512c9-a75e-4508-a7dd-e99f9e56524a)

      * In order for NFS server to be accessible from your client, you must also open following ports: TCP 111, UDP 111, UDP  2049
   
     ## Setting Up And Configuring The Database Server

    * I spinned up another EC2 instance on the AWS to serve the purpose of a database server.connected to the server from my terminal via ssh connection and performed the following commands in setting up mysql database:

       * Updating the server

       `Sudo apt update`
     
       * Upgrading the server
     
        `sudo apt upgrade`

      * Install MySQL server

        `sudo apt install mysql-server`

        ![image](https://github.com/Mubarokahh/DevOps-Projects/assets/135038657/5789a7d3-0c38-4e21-a5d9-a59c793bf408)

      *  Create a database and name it tooling
      *  Create a database user and name it webaccess
      *  Grant permission to webaccess user on tooling database to do anything only from the webservers subnet cidr
     
        mysql> CREATE DATABASE tooling;
        mysql> CREATE USER ‘webaccess’@’172.31.80.0/20’ IDENTIFIED BY ‘mypassword’;
        mysql> GRANT ALL PRIVILEGES ON ‘tooling’.* TO ‘webaccess’@’172.31.80.0/20’;


      ![image](https://github.com/Mubarokahh/DevOps-Projects/assets/135038657/644c903a-d73e-42e2-8fa4-2df58751663f)

      ## Prepare the Web Servers

      We need to make sure that our Web Servers can serve the same content from shared storage solutions, in our case – NFS Server and        MySQL database.
      You already know that one DB can be accessed for reads and writes by multiple clients. For storing shared files that our Web            Servers will use – we will utilize NFS and mount previously created Logical Volume lv-apps to the folder where Apache stores            files to be served to the users (/var/www).

      This approach will make our Web Servers stateless, which means we will be able to add new ones or remove them whenever we need,         and the integrity of the data (in the database and on NFS) will be preserved.

    * Launche 3 EC2 instances with RHEL 8 Operating System on the AWS which will serve as servers.
    * Update and upgrade the three servers.

        `sudo yum update -y`
      
    * Install nfs clients on the 3 severs:$

      `sudo yum install nfs-utils nfs4-acl-tools –y`

      ![image](https://github.com/Mubarokahh/DevOps-Projects/assets/135038657/a4fe561e-63df-447d-bea4-a12a095c8636)

      
    N0TE: I will be using one out of the three server as pictorial reference for the activities.{WEBSERVER 1}
    
    * Create the directory where to serve our devops tooling web content
      `sudo mkdir /var/www`
      
      
    * Target the NFS exports for apps with the NFS server’s private IP address and mounting it on /var/www directory:
   
      `sudo mount -t nfs -o rw,nosuid 172.31.92.26:/mnt/apps /var/www`
      
    *
    *
    *
    *
    






     
        


         








       




        




      




  
