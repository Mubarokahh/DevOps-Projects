# TOOLING WEBSITE DEPLOYMENT AUTOMATION WITH CONTINUOUS INTEGRATION

In previous Project 8 we introduced horizontal scalability concept, which allow us to add new Web Servers to our Tooling Website and you have successfully deployed a set up with 2 Web Servers and also a Load Balancer to distribute traffic between them. If it is just two or three servers – it is not a big deal to configure them manually. Imagine that you would need to repeat the same task over and over again adding dozens or even hundreds of servers.

DevOps is about Agility, and speedy release of software and web solutions. One of the ways to guarantee fast and repeatable deployments is Automation of routine tasks

DevOps is about Agility, and speedy release of software and web solutions. One of the ways to guarantee fast and repeatable deployments is Automation of routine tasks

In this project we are going to start automating part of our routine tasks with a free and open source automation server – Jenkins. It is one of the most popular CI/CD tools, it was created by a former Sun Microsystems developer Kohsuke Kawaguchi and the project originally had a named “Hudson”

According to Circle CI, Continuous integration (CI) is a software development strategy that increases the speed of development while ensuring the quality of the code that teams deploy. Developers continually commit code in small increments (at least daily, or even several times a day), which is then automatically built and tested before it is merged with the shared repository.


in this project, we will Enhance the architecture prepared in Project 8 by adding a Jenkins server, configure a job to automatically deploy source codes changes from Git to NFS .

Here is how your updated architecture will look like upon competition of this project:

![image](https://github.com/Mubarokahh/DevOps-Projects/assets/135038657/6576c1b8-4ecb-4efb-a0bc-35cbfd386738)


## Install and configure Jenkins Server

 * Create an AWS EC2 server based on Ubuntu Server 20.04 LTS and name it “Jenkins”

  ![image](https://github.com/Mubarokahh/DevOps-Projects/assets/135038657/53f905fb-9b39-40e3-a592-e4467f48f5cf)

 * Install JDK (since Jenkins is a Java-based application)

   `sudo apt update`

   `sudo  apt install default-jdk-headless`
   
 ![image](https://github.com/Mubarokahh/DevOps-Projects/assets/135038657/92c26718-19a7-44ec-97c0-201f095a13e0)

 * Obtain the Jenkins Key

    `wget -q -O - https://pkg.jenkins.io/debian-stable/jenkins.io.key | sudo apt-key add -
    sudo sh -c 'echo deb https://pkg.jenkins.io/debian-stable binary/ > \
   /etc/apt/sources.list.d/jenkins.list'
   sudo apt update
   sudo apt-get install jenkins`
   
   ![image](https://github.com/Mubarokahh/DevOps-Projects/assets/135038657/24d2caef-2918-4d21-a80d-8580f2e31223)


   * Check if Jenkins is up and running
  
     `sudo systemctl status jenkins`

   ![image](https://github.com/Mubarokahh/DevOps-Projects/assets/135038657/2d18bd4f-8120-449f-8308-644027015a0d)


  * By default Jenkins server uses TCP port 8080 – open it by creating a new Inbound Rule in your EC2 Security Group

  * Perform initial Jenkins setup
    
      From your browser access

    `http://<Jenkins-Server-Public-IP-Address-or-Public-DNS-Name>:8080`

    You will be prompted to provide a default admin password

    ![image](https://github.com/Mubarokahh/DevOps-Projects/assets/135038657/e185ebe3-57bc-4fc5-99ed-b294f8dacb3f)

  * Retrieve the password from your server running

     `sudo cat /var/lib/jenkins/secrets/initialAdminPassword`

 * Then you will be asked which plugins to install – choose suggested plugins.

 ![image](https://github.com/Mubarokahh/DevOps-Projects/assets/135038657/e1259799-1017-49e1-b77d-61a995c32337)

 Once plugins installation is done – create an admin user and you will get your Jenkins server address.

 ![image](https://github.com/Mubarokahh/DevOps-Projects/assets/135038657/2ccb3be7-32b0-422e-bbdf-42f9e4dbb1da)


 ![image](https://github.com/Mubarokahh/DevOps-Projects/assets/135038657/0fdbf882-3ece-482e-9610-384a184deef0)


![image](https://github.com/Mubarokahh/DevOps-Projects/assets/135038657/705c159d-3f8a-47bc-93ea-4b1cd5ecd011)


![image](https://github.com/Mubarokahh/DevOps-Projects/assets/135038657/edc7a853-d9c4-4a69-81b1-28004928d6db)

![image](https://github.com/Mubarokahh/DevOps-Projects/assets/135038657/4636672d-dc0c-4adb-b957-661da18d7e9f)

##  Configure Jenkins to retrieve source codes from GitHub using Webhooks

In this part, you will learn how to configure a simple Jenkins job/project (these two terms can be used interchangeably). This job will will be triggered by GitHub webhooks and will execute a ‘build’ task to retrieve codes from GitHub and store it locally on Jenkins server






   

  



