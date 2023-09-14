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


 ## Configuring Jenkins build job to save the content of the repository whenever a change is made.

* Creating a new repository named ansible-config-mgt in my github account

  ![image](https://github.com/Mubarokahh/DevOps-Projects/assets/135038657/b54f80ad-1804-412c-956b-9f51dc557a8b)
  
* Configured a Webhook in GitHub and set webhook to trigger ansible build

  ![image](https://github.com/Mubarokahh/DevOps-Projects/assets/135038657/0dd16032-e9e0-4c0d-8567-d00ec045edbe) 

* Creating  a new freestyle project ansible,pointing it to ansible-config-mgt repository.

  ![image](https://github.com/Mubarokahh/DevOps-Projects/assets/135038657/3439c24f-d844-49d2-ab77-9a05772dd220)

  ![image](https://github.com/Mubarokahh/DevOps-Projects/assets/135038657/d2c9f143-7631-4e70-8377-872fe3ce68bc)

* Configuring a Post-build job to save all (**) files

  ![image](https://github.com/Mubarokahh/DevOps-Projects/assets/135038657/0be96edb-68f7-4a63-ba61-52a88e5c2666)

  ![image](https://github.com/Mubarokahh/DevOps-Projects/assets/135038657/a7f70478-70cc-43b5-b041-0d6e19ab29d7)

* Made changes to the README.md file in the Ansible-config-mgt repo to verify the configuration,making sure that the build started aothomatically.

  ![image](https://github.com/Mubarokahh/DevOps-Projects/assets/135038657/bf910a09-70d6-4d26-aa4f-d68df30982ff)

  ![image](https://github.com/Mubarokahh/DevOps-Projects/assets/135038657/74e7d85c-c547-48f4-86c3-242843b3fd8a)


* Verifying the location of the stored artifacts:

  `ls /var/lib/jenkins/jobs/ansible/builds/<build_number>/archive`
 ![image](https://github.com/Mubarokahh/DevOps-Projects/assets/135038657/0a390698-1719-4215-8f35-046d0fa12157)

**SIDENOTE**
The diagram below shows the visual representation of what the architecture looks like now.

![image](https://github.com/Mubarokahh/DevOps-Projects/assets/135038657/5a2985f0-ad00-41ba-8787-4c7d8dee72bc)

## Setting up visual studio code as the integrated development environment 
A Visual Studio Code is an open-source code editor that integrates with popular version control systems like Git, providing visual diffs, conflict resolution,and Git repository management directly within the editor.

* Configure VSC to connect to the new created reposotory (Ansible-config-management)

  ![image](https://github.com/Mubarokahh/DevOps-Projects/assets/135038657/c6c3946c-9185-403c-b4a5-e0d36024a87b)

* Clone the Ansible-config-mgt repository to the Jenkins-Ansible server

  `git clone  <repository address>`

  ![image](https://github.com/Mubarokahh/DevOps-Projects/assets/135038657/220a0a4f-1c29-4374-9ed1-4b15060434e1)

## Ansible Development

* in the Ansible-config-mgt repository,create new branch to be used for the development of a new feature.I named the branch **Feature-11**

  
![image](https://github.com/Mubarokahh/DevOps-Projects/assets/135038657/61541590-538c-459d-b5ff-3469137a9731)

* Checkout the newly created feature branch to your local machine and start building your code and directory structure

* Create a directory and name it playbooks – it will be used to store all your playbook files
  
* Create a directory and name it inventory – it will be used to keep your hosts organised

* Within the playbooks folder, create the first playbook, and name it common.yml

* Within the inventory folder, create an inventory file (.yml) for each environment (Development, Staging Testing and Production) dev, staging, uat, and prod respectively.

* Save below inventory structure in the inventory/dev file to start configuring your development servers. Ensure to replace the IP addresses according to your own setup

 ` [nfs]
<NFS-Server-Private-IP-Address> ansible_ssh_user='ec2-user'
[webservers]
<Web-Server1-Private-IP-Address> ansible_ssh_user='ec2-user'
<Web-Server2-Private-IP-Address> ansible_ssh_user='ec2-user'
[db]
<Database-Private-IP-Address> ansible_ssh_user='ec2-user' 
[lb]
<Load-Balancer-Private-IP-Address> ansible_ssh_user='ubuntu' `

![image](https://github.com/Mubarokahh/DevOps-Projects/assets/135038657/039d3f03-2c7d-43c4-935c-5655b1feef4a)


## Implementing SSH-Agent On Jenkins-Ansible Server

* Ansible uses TCP port 22 by default,which means it needs to ssh into target servers **nfs, db, webservers, lb**

* Importing the private key into the ssh-agent with the command below
  
	`eval ssh-agent -s`
 
  	`ssh-add <path-to-private-key>`

* ssh into the Jenkins-Ansible server using ssh-agent

  `ssh -A ubuntu@public-ip`
  
  ![image](https://github.com/Mubarokahh/DevOps-Projects/assets/135038657/7242bbc1-29bd-48c9-9e9e-18de9f2e3bac)

* Confirm the key has been added with the command below, you should see the name of your key

  `ssh-add -l`
  
  ![image](https://github.com/Mubarokahh/DevOps-Projects/assets/135038657/e34142d5-2051-421d-a9f0-082964251e7c)


 ## CREATE A COMMON PLAYBOOK

 * Input the following code to the common.yml file .

```
- name: update web and nfs aservers
  hosts: webservers, nfs
  remote_user: ec2-user
  become: yes
  become_user: root
  tasks:
    - name: ensure wireshark is at the latest version
      yum:
        name: wireshark
        state: latest

- name: update LB server
  hosts: lb, db
  remote_user: ubuntu
  become: yes
  become_user: root
  tasks:
    - name: Update apt repo
      apt: 
        update_cache: yes

    - name: ensure wireshark is at the latest version
      apt:
        name: wireshark
        state: latest

- name: For all servers
  hosts: all
  remote_user: root
  become: yes
  tasks:
    - name: Set timezone to Africa/Lagos
      community.general.timezone:
        name: Africa/Lagos
   
    - name: Create a directory
      ansible.builtin.file:
        path: /etc/tmp/shell_folder
        state: directory
        mode: '0777'

    - name: Create a file
      ansible.builtin.file:
        path: /etc/tmp/shell_folder/just_file.sh
        state: touch
        mode: '0777'
        
    - name: Execute a script
      script: /etc/tmp/shell_folder/just_file.sh`
```
## Update git with the latest code

* Commit code into git repository.use git commands to add, commit and push your branch to GitHub

  `git status`

  `git add <selected files>`

  `git commit -m "commit message"`


  ![image](https://github.com/Mubarokahh/DevOps-Projects/assets/135038657/98ef829d-f7ad-4721-b9e7-d33dc74c01c2)

* Create a Pull request (PR)
* checkout from the feature branch into the master, and pull down the latest changes.

![image](https://github.com/Mubarokahh/DevOps-Projects/assets/135038657/cb876740-5d59-4909-8284-2c040e747fde)

## Run first Ansible test

* Now, it is time to execute ansible-playbook command and verify if your playbook actually works:

 `cd ansible-config-mgt`
  
 `ansible-playbook -i inventory/dev.yml playbooks/common.yml`

  




  



  










  










