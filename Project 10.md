# Load Balancer Solution With Nginx and SSL/TLS (Transport Layer Security)
 In this project , we will configure Nginx load balance solution.It is extremely important to ensure that conection to your Web solutions are secure and information is encrypted in transit, we will also cover connection over secured HTTP (HTTPS protocol), its purpose and what is required to implement it.
 When data is moving between a client (browser) and a Web Server over the Internet.it passes through multiple network devices and, if the data is not encrypted, it can be relatively easy intercepted by someone who has access to the intermediate equipment. This kind of information security threat is called Man-In-The-Middle (MIMT) attack.
 This threat is real – users that share sensitive information (bank details, social media access credentials, etc.) via non-secured channels, risk their data to be compromised and used by cybercriminals.

 SSL and its newer version, TSL – is a security technology that protects connection from MITM attacks by creating an encrypted session between browser and Web server. Here we will refer this family of cryptographic protocols as SSL/TLS – even though SSL was replaced by TLS, the term is still being widely used

SSL/TLS uses digital certificates to identify and validate a Website. A browser reads the certificate issued by a Certificate Authority (CA) to make sure that the website is registered in the CA so it can be trusted to establish a secured connection.

## Task

## This project consists of two parts:

* Configure Nginx as a Load Balancer
  
* Register a new domain name and configure secured connection using SSL/TLS certificates
  
Your target architecture will look like this:

![image](https://github.com/Mubarokahh/DevOps-Projects/assets/135038657/20754d9b-2824-4589-a412-e21c1476dfa9)

## Configure Nginx as a load balancer

* Create an EC2 VM based on Ubuntu Server 20.04 LTS and name it Nginx LB (do not forget to open TCP port 80 for HTTP connections, also open TCP port 443 – this port is used for secured HTTPS connections)
* Update /etc/hosts file for local DNS with Web Servers’ names (e.g. Web1 and Web2) and their local IP addresses.
* Install and configure Nginx as a load balancer to point traffic to the resolvable DNS names of the webservers

  ![image](https://github.com/Mubarokahh/DevOps-Projects/assets/135038657/0a0775ce-52bd-42c6-83e3-61cc08a159bd)

  ![image](https://github.com/Mubarokahh/DevOps-Projects/assets/135038657/d837f425-3a07-4009-bad1-ed56e9f36786)


* Update the instance and Install Nginx

  `sudo apt update`

  `sudo apt install nginx`

  ![image](https://github.com/Mubarokahh/DevOps-Projects/assets/135038657/3831f4cf-4068-44bf-bdfd-43afbd7146e5)

* Configure Nginx LB using Web Servers’ names defined in /etc/hosts

  ![image](https://github.com/Mubarokahh/DevOps-Projects/assets/135038657/e0c7a88d-09a5-4224-87ce-e4c3010f673b)


* Open the congiuration file by running:
    
    `sudo vi /etc/nginx/nginx.conf`

* Insert the following configuration onto the http set up:

   ` upstream myproject {
    server Web1 weight=5;
    server Web2 weight=5;
  }
server {
    listen 80;
    server_name www.domain.com;
    location / {
      proxy_pass http://myproject;
    }
  }
`
 ![image](https://github.com/Mubarokahh/DevOps-Projects/assets/135038657/371e56bf-b9f5-4174-99a2-db0d60fd77b1)

* Restart Nginx and make sure the service is up and running

   `sudo systemctl restart nginx`

   `sudo systemctl status nginx`

  ![image](https://github.com/Mubarokahh/DevOps-Projects/assets/135038657/cc392440-1ae4-4ba3-87f4-9d1a1653ded6)

  ## Register a new domain name and configure secured connection using SSL/TLS certificates

  * Register a new domain name with any registrar of your choice in any domain zone (e.g. .com, .net, .org, .edu, .info, .xyz or any other)
  * Assign an Elastic IP to your Nginx LB server and associate your domain name with this Elastic IP
  * Update A record in your registrar to point to Nginx LB using Elastic IP address
 
  * Configure nginx to recognize my new domain name:
    
     `sudo vi /etc/nginx/nginx.conf`
 
    ![image](https://github.com/Mubarokahh/DevOps-Projects/assets/135038657/96ac6a2d-d76d-4f44-8c39-0e806a114fb1)

  ## Install certbot and request for an SSL/TLS certificate

  * Make sure snapd service is active and running to install snapd
 
    `sudo systemctl status snapd`

    ![image](https://github.com/Mubarokahh/DevOps-Projects/assets/135038657/b316b56a-5c3c-4f23-9ba8-4ca5cd2fe2b0)
    
  * Install certbot to be  used to request for SSL/TLS certificate
    
    `sudo snap install --classic certbot`
    
   ![image](https://github.com/Mubarokahh/DevOps-Projects/assets/135038657/c9ec1b6a-ec06-49ee-a04d-3729e26de52a)

  * create a Request your certificate for the domain name.
 
    `sudo ln -s /snap/bin/certbot /usr/bin/certbot`

  * Selecting domain name

    `sudo certbot --nginx`








