# Simulating A Typical CI/CD Pipeline For A PHP Based Application

The CI/CD concept is applied in this project by pushing a PHP application from Github to Jenkins to perform a multi-branch pipeline job (build job is done on each branches of a repository simultaneously) . To ensure continuous integration of codes from many developers, this is done. Following that, the build job's artifacts are packaged and sent to a sonarqube server for testing before being deployed to an artifactory, where ansible is used to trigger a job that will deploy the application to a production environment.The CI/CD pipeline is represented by the diagram below.

 ![image](https://github.com/Mubarokahh/DevOps-Projects/assets/135038657/84540039-1393-49e3-855a-4381e0bca799)

* Configuring Ansible-jenkins Server
  
  ![image](https://github.com/Mubarokahh/DevOps-Projects/assets/135038657/16e51a51-3aa9-44ac-8281-8c2ed1d8a062)

* Installing Git to the server
  
  ![image](https://github.com/Mubarokahh/DevOps-Projects/assets/135038657/6ca396a3-7dc6-4f37-8916-80e5eca5f28d)


* Cloning ansible-config-mgt repository into the server

 ![image](https://github.com/Mubarokahh/DevOps-Projects/assets/135038657/c6e7df7e-ac73-49fc-9c1b-1c29274556ab)

* Install Epel,Remi Repo Release and Java

  ![image](https://github.com/Mubarokahh/DevOps-Projects/assets/135038657/6188fceb-c41f-457f-a1fd-d4a446155ee0)

* Jenkins is Ready

  ![image](https://github.com/Mubarokahh/DevOps-Projects/assets/135038657/f9871961-54f6-49fa-8814-575cfa4201d2)

* Configuring Jenkins-Ansible serverfor jenkins deployment

 I have been launching Ansible commands manually from a CLI in previous project, in this project i will be runnig ansible commands from the jenkins UI. To achive this, the following steps will be taken:

* Navigate to jenkins URL
* Install Blue Ocean plugin from manage plugins on Jenkins:
    
  ![image](https://github.com/Mubarokahh/DevOps-Projects/assets/135038657/452a16a2-e90c-48d5-b9c0-1c1390430ac5)
  
* Creating new pipeline job on the Blue Ocean UI from Github

 ![image](https://github.com/Mubarokahh/DevOps-Projects/assets/135038657/e0437149-2b69-42af-89fc-29b3630e9fb4)

 ![image](https://github.com/Mubarokahh/DevOps-Projects/assets/135038657/65ac6dc6-3c04-4919-a15d-75576154dac9)

 ![image](https://github.com/Mubarokahh/DevOps-Projects/assets/135038657/1a70a28d-11f4-4802-8682-979951dd4a48)

NOTE: At this point, their is no jenkins file present in the ansible repository.Although,Blue Ocean will attempt to give you some guidance to create one but this will be disregarded.I will create the jenkinsfile.

This is the newly created pipeline.The piprline takes the name of the github repository.
 ![image](https://github.com/Mubarokahh/DevOps-Projects/assets/135038657/69c23759-6e86-45cf-bf6c-e1a60612a34a)

* Creating a jenkinsfile
  Going back to the ansible project, i will create a new directory deploy and start a new file Jenkinsfile inside the directory.
  
 ![image](https://github.com/Mubarokahh/DevOps-Projects/assets/135038657/e2e34dad-4610-4c4e-91b4-f2f0e1df2406)

 * Adding the code snippet below to start building the Jenkinsfile gradually. This pipeline currently has just one stage called Build and the only thing we are doing is using the shell script module to echo Building Stage.

 ```
   pipeline {
    agent any
  stages {
    stage('Build') {
      steps {
        script {
          sh 'echo "Building Stage"'
        }
      }
    }
    }
}
```















