# ANSIBLE DYNAMIC ASSIGNMENTS (INCLUDE) AND COMMUNITY ROLES

## Introduction 
static assignment was explored using "imports" module. This project will be centered around dynamic assignments which uses "Include" module
  
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

![image](https://github.com/Mubarokahh/DevOps-Projects/assets/135038657/6988ca9b-e68d-4c58-b550-ef9569b44eff)

* Updating the changes into GitHub

```
git add .
git commit -m "Commit new role files into GitHub"
git push --set-upstream origin roles-feature

```

![image](https://github.com/Mubarokahh/DevOps-Projects/assets/135038657/4744732f-d55e-4d86-97fa-91f7c511b9a8)

## Implementing Load Balancer(Apache & Nginx) Roles
* Two load-balancers are set up up which are APACHE and NGINX. Since both Nginx and Apache load balancer cannnot be used, a condition has to be addedto enable either one – this is where you can make use of variables.

* Creating  apache role in the role directory
  
  `sudo ansible-galaxy init apache`

* Creating nginx role in the role directory

  `sudo ansible-galaxy init nginx`

  ![image](https://github.com/Mubarokahh/DevOps-Projects/assets/135038657/e6be869c-b7df-4cb9-a218-0e89ce1354cf)

  ![image](https://github.com/Mubarokahh/DevOps-Projects/assets/135038657/95c780bd-80c8-4c01-9ff9-79c07b80788b)

* Declaring the following variable in the  defaults/main.yml file of both apache and nginx roles file which makes ansible to skip the roles during execution.


* Declaring another variable in both roles `load_balancer_is_required` and set its value to `false` as well

  ![image](https://github.com/Mubarokahh/DevOps-Projects/assets/135038657/4d3aea93-a578-4e54-86fa-cacbd13f37cc)

  ![image](https://github.com/Mubarokahh/DevOps-Projects/assets/135038657/0a2d79a4-e773-4022-88af-05b824c79c82)

* Updating both assignment and site.yml files respectively

* Site.yml

```
 - name: Loadbalancers assignment
       hosts: lb
         - import_playbook: ../static-assignments/loadbalancers.yml
        when: load_balancer_is_required 
```

![image](https://github.com/Mubarokahh/DevOps-Projects/assets/135038657/6cc7310e-ed78-427d-a556-e7426b69abe3)


* Loadbalancers.yml

```
  - hosts: lb
  roles:
    - { role: nginx, when: enable_nginx_lb and load_balancer_is_required }
    - { role: apache, when: enable_apache_lb and load_balancer_is_required }
```
![image](https://github.com/Mubarokahh/DevOps-Projects/assets/135038657/0f192656-53e4-40ea-b23b-6354ac5ea41a)

* To define which load balancer to use, the files in the env-var folder is used to override the default settings of any of the load balancer roles. In this case the env-var/dev.yml file is used to make ansible to only run nginx load balancer task in the target server:

`enable_nginx_lb: true
load_balancer_is_required: true`

![image](https://github.com/Mubarokahh/DevOps-Projects/assets/135038657/f054a272-9756-41e1-bd3b-6c6c8537305f)

* Running the playbook 
 ` ansible-playbook -i inventory/uat.yml playbooks/site.yml`
![image](https://github.com/Mubarokahh/DevOps-Projects/assets/135038657/2d497560-6622-4d26-a34c-0f4c378713f3)
![image](https://github.com/Mubarokahh/DevOps-Projects/assets/135038657/114db901-6b0b-4ade-98c7-572ab7feadfc)

NOTE: In the above play,I activated loadbalancer and enabled nginx,Notice how it skips all apache2 plays, this is because nginx load balancer is activated.

* The play below skips ngnix plays, this is beacause apache load balancer is activated.

![image](https://github.com/Mubarokahh/DevOps-Projects/assets/135038657/276756e2-5722-4ef2-8a2d-a6d717747d56)








`






## Error Encountered during the implementation of this project

## Error Code

While trying to install the mysql in the `~/ansible-config-mgt/role`directory  using the community role `role ansible-galaxy install geerlingguy.mysql`. The role authomatically created a folder in the `~/ansible-config-mgt` directory as

## Error Execution

 `roles'$'\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n''deprecation_warnings = False'`

![image](https://github.com/Mubarokahh/DevOps-Projects/assets/135038657/591acf3d-60b5-48ae-b077-f290f73a7e47)

## Fix Execution
   Because I was using ansible-galaxy command to install a role, and by default, it installs roles in the system-wide location, typically in a directory like /etc/ansible/roles or ~/.ansible/roles.
   However, because I want to manage my roles within your project directory, which is a good practice when working with roles that are specific to my project.
  To have the role installed in my project directory, i specified the role path when installing the role by running;

## Fix Code
  `sudo ansible-galaxy install geerlingguy.mysql -p ~/ansible-config-mgt/roles`











