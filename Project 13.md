# ANSIBLE DYNAMIC ASSIGNMENTS (INCLUDE) AND COMMUNITY ROLES
## Introduction 
  This project is a continuation of project 12.
  In project 12,static assignment was explored using "imports" module. This project will be centered around dynamic assignments which uses "Include" module
  
##  Introducing dynamic assignments into the existing structure
* Creating a new folder in the https://github.com/Mubarokahh/ansible-config-mgt.git called 'DYNAMIC-ASSIGNMENTS'
* Creting an environment varaiable file in the dynamic-assignments named 'env-vars.yml'

  ![image](https://github.com/Mubarokahh/DevOps-Projects/assets/135038657/c8de39eb-7b2e-485f-bb2d-5ebea7378e4a)

* Creating a folder that holds the environmental variable and naming it ‘env-var’
* Creating the following files in the ‘env-var’: dev.yml, uat.yml, prod.yml and stage.yml
 Ansible-config-mgt file structure will look like this

![image](https://github.com/Mubarokahh/DevOps-Projects/assets/135038657/05922fcf-fdb5-41a2-afc3-d930dabf21aa)

* The following code will be pasted into env-vars.yml which exists in the dynamic environment

```
---
- name: collate variables from env specific file, if it exists
  hosts: all
  tasks:
    - name: looping through list of available files
      include_vars: "{{ item }}"
      with_first_found:
        - files:
            - dev.yml
            - stage.yml
            - prod.yml
            - uat.yml
          paths:
            - "{{ playbook_dir }}/../env-vars"
      tags:
        - always

   ```

 ![image](https://github.com/Mubarokahh/DevOps-Projects/assets/135038657/9cc8c194-8afc-47ad-9617-5889272bc8f8)

 * Updating site.yml to work with dynamic assignments

  ```
 ---
- hosts: all
- name: Include dynamic variables 
  tasks:
  import_playbook: ../static-assignments/common.yml 
  include: ../dynamic-assignments/env-vars.yml
  tags:
    - always

-  hosts: webservers
- name: Webserver assignment
  import_playbook: ../static-assignments/webservers.yml
```

![image](https://github.com/Mubarokahh/DevOps-Projects/assets/135038657/d1ffcc9e-d14a-4428-b8da-419707c0dfc8)

## Implementing community role

To preserve my GitHub in its actual state after implementing a role in the bastion server (ansible-jenkins), I made use of git commands so I can easily commit the changes made and pushing it to the ansible-config-mgt repository directly from the bastion server.

 * The bastion server was configured accordingly
 
 ```
git init
git pull https://github.com/<your-name>/ansible-config-mgt.git
git remote add origin https://github.com/<your-name>/ansible-config-mgt.git
git branch roles-feature
git switch roles-feature

```


![image](https://github.com/Mubarokahh/DevOps-Projects/assets/135038657/f789b8fe-b1a6-440c-bd05-273513575090)

![image](https://github.com/Mubarokahh/DevOps-Projects/assets/135038657/625d646c-8e22-47dd-9753-38ba1235e66f)


* In role directory, install MySQL role using community `role ansible-galaxy install geerlingguy.mysql`

`mv geerlingguy.mysql/ mysql`

![image](https://github.com/Mubarokahh/DevOps-Projects/assets/135038657/2882d9eb-98bd-4eeb-8d30-9123e089c5aa)



## Error Encountered during the implementation of this project

While trying to install the mysql in the `~/ansible-config-mgt/role`directory  using the community role `role ansible-galaxy install geerlingguy.mysql`. The role authomatically created a folder in the `~/ansible-config-mgt` directory as `roles'$'\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n''deprecation_warnings = False'`

![image](https://github.com/Mubarokahh/DevOps-Projects/assets/135038657/591acf3d-60b5-48ae-b077-f290f73a7e47)

* Troubleshooting Process
   Because I was using ansible-galaxy command to install a role, and by default, it installs roles in the system-wide location, typically in a directory like /etc/ansible/roles or ~/.ansible/roles.
   However, because I want to manage my roles within your project directory, which is a good practice when working with roles that are specific to my project.
  To have the role installed in my project directory, i specified the role path when installing the role by running;
  
  `sudo ansible-galaxy install geerlingguy.mysql -p ~/ansible-config-mgt/roles`











