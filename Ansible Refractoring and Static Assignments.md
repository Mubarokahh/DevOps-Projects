# Ansible Refractoring and Static Assignments (Imports and Roles)
The ansible-config-mgt repository se up in the project 11 will for this project. However, some improvement will be made to the code.The ansible code will be refractored, assignments wull be created and impoerts functionality will also be explpored.Imports allow to effectively re-use previously created playbooks in a new playbook – it allows to organize your tasks and reuse them when needed.

## Jenkins Job Enhancement
Because artifacts on the Jenkins server change directory with each build. To maintain track of current artefacts, the copy-artifacts plugin is used to copy recent artifacts from a build and automatically paste them to a defined directory where ansible playbooks may be easily launched.

* Create a new directory in the ansible-jenkins server  called ansible-config-artifact. artifacts will be stored there after each build.
* Change permissions to this directory, so Jenkins could save files there
  
 ![image](https://github.com/Mubarokahh/DevOps-Projects/assets/135038657/dc787265-7efb-488b-a728-b3a1766b3cfc)

* Go to Jenkins web console -> Manage Jenkins -> Manage Plugins -> on Available tab search for Copy Artifact and install this plugin without restarting Jenkins

  ![image](https://github.com/Mubarokahh/DevOps-Projects/assets/135038657/f2e28a3b-ae1e-4226-b8bf-8be27cbab421)

* Create a new Freestyle project and name it save_artifacts.
  
* his project will be triggered by completion of your existing ansible project.
  
![image](https://github.com/Mubarokahh/DevOps-Projects/assets/135038657/790c55ca-dd47-4e46-a39b-54ab0868497c)

![image](https://github.com/Mubarokahh/DevOps-Projects/assets/135038657/5f17c663-9580-4259-ab89-289ba0b05f6a)

* The main idea of save_artifacts project is to save artifacts into /home/ubuntu/ansible-config-artifact directory. To achieve this, create a Build step and choose Copy artifacts from other project, specify ansible as a source project and /home/ubuntu/ansible-config-artifact as a target directory.

  ![image](https://github.com/Mubarokahh/DevOps-Projects/assets/135038657/01b7d043-3116-4971-baa3-7e38bdfce4b9)

* Testing the configuration by making a change in the README.md file

  ![image](https://github.com/Mubarokahh/DevOps-Projects/assets/135038657/38260af4-a739-4232-8b52-c4fdb440cd6a)

  ![image](https://github.com/Mubarokahh/DevOps-Projects/assets/135038657/9ecfbd2a-bb33-4f1f-833d-f07ffc9ec03d)

## Refactor Ansible code by importing other playbooks into site.yml

* Within playbooks folder, create a new file and name it site.yml. The site.yml file will now be considered as an entry point into the entire infrastructure configuration.

* Create a new folder in root of the repository and name it static-assignments. The static-assignments folder is where all other children playbooks will be stored

* Move common.yml file into the newly created static-assignments folder.

![image](https://github.com/Mubarokahh/DevOps-Projects/assets/135038657/f2f8c289-3446-4abf-9f35-3010631f390e)

* Inside site.yml file, import common.yml playbook.

```
 ---
- hosts: all
- import_playbook: ../static-assignments/common.yml

```
  ![image](https://github.com/Mubarokahh/DevOps-Projects/assets/135038657/74a1cc21-c5ef-4ea9-8a3f-a38839af15b2)

* The ansible-config-mgt folder structure

  ![image](https://github.com/Mubarokahh/DevOps-Projects/assets/135038657/fc1e1b2d-5817-457a-a39b-94f150bdaaaa)

* Create another playbook under static-assignments and name it common-del.yml.
* Configure the deletion of wireshark utility in this playbook


```  
---
   - name: update web, nfs and db servers
    hosts: webservers, nfs
    remote_user: ec2-user
  become: yes
  become_user: root
  tasks:
  - name: delete wireshark
    yum:
      name: wireshark
      state: removed

- name: update LB server
  hosts: lb, db
  remote_user: ubuntu
  become: yes
  become_user: root
  tasks:
  - name: delete wireshark
    apt:
      name: wireshark-qt
      state: absent
      autoremove: yes
      purge: yes
      autoclean: yes
```


* Update site.yml with - import_playbook: ../static-assignments/common-del.yml instead of common.yml

 ![image](https://github.com/Mubarokahh/DevOps-Projects/assets/135038657/c4b1790a-4e48-4746-954a-82b549ae31d0)

* Run ansible-playbook command against the dev environment

` cd /home/ubuntu/ansible-config-mgt/`

`ansible-playbook -i inventory/dev.yml playbooks/site.yaml`

 ![image](https://github.com/Mubarokahh/DevOps-Projects/assets/135038657/6182fb2c-be15-4c92-9efc-0420b2e7b475)




## Configure UAT Webservers with a role ‘Webserver’

* Launch 2 fresh EC2 instances using RHEL 8 image, we will use them as our uat servers,namely Web1-UAT and Web2-UAT

  ![image](https://github.com/Mubarokahh/DevOps-Projects/assets/135038657/1a106b5c-2462-40e9-ae95-ffe56d53c59f)
  
* Creating a role:
   ** Create a directory called roles,relative to the playbook file or in /etc/ansible/ directory.

## NOTE:

 There are two ways how you can create this folder structure:

* Use an Ansible utility called ansible-galaxy inside ansible-config-mgt/roles directory (you need to create roles directory upfront)

  `mkdir roles`
  
`cd roles`

`ansible-galaxy init webserver`


* Create the directory/files structure manually

  ![image](https://github.com/Mubarokahh/DevOps-Projects/assets/135038657/5b0157a2-2899-43d7-afa6-1cc48707ac5c)


## The Webserver Role folder structure

  ![image](https://github.com/Mubarokahh/DevOps-Projects/assets/135038657/31cb1428-e4ca-457c-95fa-07013ed87182)

```
[uat-webservers]
<Web1-UAT-Server-Private-IP-Address> ansible_ssh_user='ec2-user' 

<Web2-UAT-Server-Private-IP-Address> ansible_ssh_user='ec2-user'

 ```

* To make ansible locate the role directory, editing the role section and specifying the role path in the ansible.cfg file

 `sudo vi /etc/ansible/ansible.cfg`

* Setting up the task file of the webserver role and adding the following task in the main.yml

  `sudo vi ansible-config-artifact/playbooks/role/webserver/task/main.yml`
  
```
---
- name: install apache
  become: true
  ansible.builtin.yum:
    name: "httpd"
    state: present
- name: install git
  become: true
  ansible.builtin.yum:
    name: "git"
    state: present
- name: clone a repo
  become: true
  ansible.builtin.git:
    repo: https://github.com/Mubarokahh/tooling.git
    dest: /var/www/html
    force: yes
- name: copy html content to one level up
  become: true
  command: cp -r /var/www/html/html/ /var/www/
- name: Start service httpd, if not started
  become: true
  ansible.builtin.service:
    name: httpd
    state: started
- name: recursively remove /var/www/html/html/ directory
  become: true
  ansible.builtin.file:
    path: /var/www/html/html
    state: absent

```



  ## The above tasks tanslate to the following;
 * Install and configure Apache (httpd service)
   
 * Clone Tooling website from GitHub https://github.com/<your-name>/tooling.git.
   
 * Ensure the tooling website code is deployed to /var/www/html on each of 2 UAT Web servers.
   
 * Make sure httpd service is started


  * ![image](https://github.com/Mubarokahh/DevOps-Projects/assets/135038657/c1320b79-d003-44cc-a6ff-32a3b7e6edbd)

  * Within the static-assignments folder, creating a new assignment for uat-webservers uat-webservers.yml. This is where the role will be referenced.

```
 ---
- hosts: uat-webservers
  roles:
     - webserver
       
```


![image](https://github.com/Mubarokahh/DevOps-Projects/assets/135038657/edb3caa0-a553-4045-9734-d15db21aadb5)

* Updating the site.yml file to be able to import uat-webservers role

 ```
---
- hosts: all
- import_playbook: ../static-assignments/common.yml

- hosts: uat-webservers
- import_playbook: ../static-assignments/uat-webservers.yml

```

 ![image](https://github.com/Mubarokahh/DevOps-Projects/assets/135038657/f0afdd59-957d-4a2e-a8df-8b217c410114)

 ## Commit & Test The changes

 `sudo ansible-playbook -i /home/ubuntu/ansible-config-mgt/inventory/uat.yml /home/ubuntu/ansible-config-mgt/playbooks/site.yml`

 ![image](https://github.com/Mubarokahh/DevOps-Projects/assets/135038657/5e72c9dd-4f59-4e74-9d5d-5ae65e0fd062)

 * Testing the outcome of the configuration in one of the webservers

  ![image](https://github.com/Mubarokahh/DevOps-Projects/assets/135038657/0e4fc49f-68bf-48a7-b48f-7b1872d285dc)


 * The configuration looks like this now

   ![image](https://github.com/Mubarokahh/DevOps-Projects/assets/135038657/2b505cb4-fe91-4191-9954-356645e59c4a)



















 ## Error Encountered during this project 
 
* Permission denied (publickey,gssapi-keyex,gssapi-with-mic,password).", "unreachable": true}


![image](https://github.com/Mubarokahh/DevOps-Projects/assets/135038657/268c30e6-46bf-4ba8-9785-dc0d5bebadd0)

## The troubleshooting process

During the implementation of this project,I encountered the above error and this got me troubleshooting for quite a while.I found out that ansible was not able to locate the private key file leading to the permission denied error. I was able to solve this error by specifying the private key path in the uat-webserver.yml for ansible to locate.

`ansible_ssh_private_key_file=/home/ubuntu/.ssh/id_rsa`

![image](https://github.com/Mubarokahh/DevOps-Projects/assets/135038657/13fd016b-bba0-4c7b-b06b-208e73b12c68)





    

      

  






