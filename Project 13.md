# ANSIBLE DYNAMIC ASSIGNMENTS (INCLUDE) AND COMMUNITY ROLES
## Introduction 
  This project is a continuation of project 12.In project 12,static assignment was explored using "imports" module. This project will be centered around dynamic assignments which uses "Include" module
  
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



