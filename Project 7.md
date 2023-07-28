
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
      

   *   Use pvcreate utility to mark each of 3 disks as physical volumes (PVs) to be used by LVM

       `sudo pvcreate /dev/xvdf1`

       `sudo pvcreate /dev/xvdg1`

       `sudo pvcreate /dev/xvdh1`


       ![image](https://github.com/Mubarokahh/DevOps-Projects/assets/135038657/555db8ac-cc26-4d86-bb2f-b8ff1950505e)


       

      




  
