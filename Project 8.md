# LOAD BALANCER SOLUTION WITH APACHE


After completing Project-7 you might wonder how a user will be accessing each of the webservers using 3 different IP address or 3 different DNS names. You might also wonder what is the point of having 3 different servers doing exactly the same thing.

When we access a website in the Internet we use an URL and we do not really know how many servers are out there serving our requests, this complexity is hidden from a regular user, but in case of websites that are being visited by millions of users per day (like Google or Reddit) it is impossible to serve all the users from a single Web Server (it is also applicable to databases, but for now we will not focus on distributed DBs)

Each URL contains a domain name part, which is translated (resolved) to IP address of a target server that will serve requests when open a website in the Internet. Translation (resolution) of domain names is perormed by DNS servers, the most commonly used one has a public IP address 8.8.8.8 and belongs to Google. You can try to query it with nslookup command:

`nslookup 8.8.8.8
Server:  UnKnown
Address:  103.86.99.99`

In our set up in Project-7 we had 3 Web Servers and each of them had its own public IP address and public DNS name. A client has to access them by using different URLs, which is not a nice user experience to remember addresses/names of even 3 server, let alone millions of Google servers

In order to hide all this complexity and to have a single point of access with a single public IP address/name, a Load Balancer can be used. A Load Balancer (LB) distributes clients’ requests among underlying Web Servers and makes sure that the load is distributed in an optimal way

Let us take a look at the updated solution architecture with an LB added on top of Web Servers (for simplicity let us assume it is a software L7 Application LB, for example – Apache, NGINX or HAProxy)


![image](https://github.com/Mubarokahh/DevOps-Projects/assets/135038657/96ffb9dd-7248-42d4-a0df-c4626a97001a)

In this project we will enhance our Tooling Website solution by adding a Load Balancer to disctribute traffic between Web Servers and allow users to access our website using a single URL.

## Configure Apache As A Load Balancer

* Create an Ubuntu Server 20.04 EC2 instance and name it Project-8-apache-lb, so your EC2 list will look like this

  ![image](https://github.com/Mubarokahh/DevOps-Projects/assets/135038657/a07c0f62-6d4c-46a7-b520-f8707f80881f)


* Open TCP port 80 on Project-8-apache-lb by creating an Inbound Rule in Security Group

  ![image](https://github.com/Mubarokahh/DevOps-Projects/assets/135038657/d6de743d-164f-4209-871c-83b53b7d5d8e)


* Install Apache Load Balancer on Project-8-apache-lb server and configure it to point traffic coming to LB to both Web Servers.

* To install apache,run the following:

  `sudo apt update`
  
  `sudo apt install apache2 -y`
  
  `sudo apt-get install libxml2-dev`

  ![image](https://github.com/Mubarokahh/DevOps-Projects/assets/135038657/3375737e-a8bd-4c3d-a785-26cad3e03254)

  * Enable the following module
 
    `sudo a2enmod rewrite`

    `sudo a2enmod proxy`

    `sudo a2enmod proxy_balancer`

    `sudo a2enmod proxy_http`

    `sudo a2enmod headers`

   ` sudo a2enmod lbmethod_bytraffic`

   ![image](https://github.com/Mubarokahh/DevOps-Projects/assets/135038657/16777bb5-e012-421c-a3f8-789dd974bc5a)

  * Restart Apache2 service

   ` sudo systemctl restart apache2`

  * Verify if Apache is running

    `sudo systemctl status apache2`

    ![image](https://github.com/Mubarokahh/DevOps-Projects/assets/135038657/a73e4c6f-a785-40a9-9da8-a5a35ccaace8)

  * Configure load balancing
 
   ` sudo vi /etc/apache2/sites-available/000-default.conf`

  * Add this configuration into this section <VirtualHost *:80>  </VirtualHost

    `<Proxy "balancer://mycluster">
  
               BalancerMember http://<WebServer1-Private-IP-Address>:80 loadfactor=5 timeout=1
               BalancerMember http://<WebServer2-Private-IP-Address>:80 loadfactor=5 timeout=1
               ProxySet lbmethod=bytraffic
               # ProxySet lbmethod=byrequests
        </Proxy>

        ProxyPreserveHost On
        ProxyPass / balancer://mycluster/
        ProxyPassReverse / balancer://mycluster/`

    ![image](https://github.com/Mubarokahh/DevOps-Projects/assets/135038657/019cd22a-2b52-4c2c-9300-fe8714bff089)


* Restart Apache2

  sudo systemctl restart apache2


 


