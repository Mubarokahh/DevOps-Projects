# AUTOMATING PROJECTS WITH ANSIBLE CONFIGURATION MANAGEMENT
 In this project,the task carried out manually from previous project (Project 7-10) will be automated using ansible configuration management.
 The jenkins server set up in [[project 9](https://github.com/Mubarokahh/DevOps-Projects/blob/main/Project%209.md)] will be configured to serve as the bastion host or jump server with the use of ansible configuration management.
Ansible client as a jump server 
A Jump Server (sometimes also referred as Bastion Host) is an intermediary server through which access to internal network can be provided. If you think about the current architecture you are working on, ideally, the webservers would be inside a secured network which cannot be reached directly from the Internet. That means, even DevOps engineers cannot SSH into the Web servers directly and can only access it through a Jump Server – it provide better security and reduces attack surface

The diagram below the Virtual Private Network (VPC) is divided into two subnets – Public subnet has public IP addresses and Private subnet is only reachable by private IP addresses.

![image](https://github.com/Mubarokahh/DevOps-Projects/assets/135038657/c8232dfc-1dd1-4cb3-a235-500ba4436471)

