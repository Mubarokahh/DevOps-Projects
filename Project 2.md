
# Project 2: LEMP STACK Implementation
LEMP STACK is a bundle of technologies used for web development. The combination of these technologies create web solutions

## Launching and connecting to EC2 environment in AWS

I launched an EC2 instance through AWS.I then launched the SSH commands on my Mac terminal and connected to the server I was operating in the AWS cloud.

image.png

## Instaliing The Nginx Wen Server
For this project,Nginx will be used to show web pages to the site's visitors.Apt package manager will be used to install this package. The following commands will be run to install Nginx.(1) sudo apt update (2) sudo apt install nginx. To verify if Nginx is running sucessfully in the virtual machine. The following syntax was run: Sudo systemtcl status Nginx

image.png 

The server running in EC2 was configured to receive traffic by the web server.hus, the EC2 was configured to open inbound connection through Port 80

image.png

To acess the server loacally from my mac termical:  curl http://localhost:80

image.png

