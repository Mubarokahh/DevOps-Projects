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

` ---
- hosts: all
- import_playbook: ../static-assignments/common.yml
`
  ![image](https://github.com/Mubarokahh/DevOps-Projects/assets/135038657/74a1cc21-c5ef-4ea9-8a3f-a38839af15b2)

* The ansible-config-mgt folder structure

  ![image](https://github.com/Mubarokahh/DevOps-Projects/assets/135038657/fc1e1b2d-5817-457a-a39b-94f150bdaaaa)

* Create another playbook under static-assignments and name it common-del.yml.
* Configure the deletion of wireshark utility in this playbook


  
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

```[uat-webservers]
<Web1-UAT-Server-Private-IP-Address> ansible_ssh_user='ec2-user' 

<Web2-UAT-Server-Private-IP-Address> ansible_ssh_user='ec2-user' 
 ```

* To make ansible locate the role directory, editing the role section and specifying the role path in the ansible.cfg file

 `sudo vi /etc/ansible/ansible.cfg`

* Setting up the task file of the webserver role and adding the following task in the main.yml

  `sudo vi ansible-config-artifact/playbooks/role/webserver/task/main.yml`

`` ` ---
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
    state: absent```

  ![image](https://github.com/Mubarokahh/DevOps-Projects/assets/135038657/a6c0871b-e469-4987-840d-87635101ee33)

    

      

  






