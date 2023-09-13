# AUTOMATING PROJECTS WITH ANSIBLE CONFIGURATION MANAGEMENT
 In this project,the task carried out manually from previous project (Project 7-10) will be automated using ansible configuration management.
 The jenkins server set up in [[project 9](https://github.com/Mubarokahh/DevOps-Projects/blob/main/Project%209.md)] will be configured to serve as the bastion host or jump server with the use of ansible configuration management.
Ansible client as a jump server 
A Jump Server (sometimes also referred as Bastion Host) is an intermediary server through which access to internal network can be provided. If you think about the current architecture you are working on, ideally, the webservers would be inside a secured network which cannot be reached directly from the Internet. That means, even DevOps engineers cannot SSH into the Web servers directly and can only access it through a Jump Server – it provide better security and reduces attack surface

The diagram below the Virtual Private Network (VPC) is divided into two subnets – Public subnet has public IP addresses and Private subnet is only reachable by private IP addresses.

![image](https://github.com/Mubarokahh/DevOps-Projects/assets/135038657/c8232dfc-1dd1-4cb3-a235-500ba4436471)

## Install and configure ansible on EC2 instance
Update the name tag of the from Jenkins EC2 instance to Jenkins-Ansible.This server will be used to run playbook.

![image](https://github.com/Mubarokahh/DevOps-Projects/assets/135038657/d4e6a769-06e9-44f4-9a79-b5754c97c4de)

* To install ansible:
  
 `sudo apt update`
 
`sudo apt install ansible`

![image](https://github.com/Mubarokahh/DevOps-Projects/assets/135038657/66fecfc1-4106-4a59-b4f5-3bfbecf399c8)

* Check the version of the ansible installed

  `ansible --version`

  ![image](https://github.com/Mubarokahh/DevOps-Projects/assets/135038657/bb97169d-ec1a-4ac6-a32c-d2981fe58072)

* Creating a new repository named ansible-config-mgt in my github account 









