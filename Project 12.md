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

