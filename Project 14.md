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

![image](https://github.com/Mubarokahh/DevOps-Projects/assets/135038657/b4fc28ba-5e53-408f-90ff-94de3ab98b99)

* Committing the changes and heading over to the Jenkins console

![image](https://github.com/Mubarokahh/DevOps-Projects/assets/135038657/879bdacd-cf21-4f19-824d-ef4518d4f372)

![image](https://github.com/Mubarokahh/DevOps-Projects/assets/135038657/9f870889-df7b-4561-ad5c-3cc127d6df91)

* Click on Configure and toogle to build configuration to set the location of the Jenkinsfile.

  ![image](https://github.com/Mubarokahh/DevOps-Projects/assets/135038657/c81331df-cfee-4fb4-a8be-f4e64735ba0c)

* Clicking on Build now to trigger the build

![image](https://github.com/Mubarokahh/DevOps-Projects/assets/135038657/014b3b9c-358b-4c11-8292-615f603a4e60)

![image](https://github.com/Mubarokahh/DevOps-Projects/assets/135038657/f2e85701-c902-4613-862f-8533e578482e)

NOTE: This pipeline is a multibranch one. This means, if there were more than one branch in GitHub, Jenkins would have scanned the repository to discover them all and we would have been able to trigger a build for each branch.

* Creating new branch to test this. Adding test stage to the build stage

![image](https://github.com/Mubarokahh/DevOps-Projects/assets/135038657/b78f9cc4-ee0b-4eec-994b-3f2767a7f724)


![image](https://github.com/Mubarokahh/DevOps-Projects/assets/135038657/fe2a9457-b699-429e-b338-5d19b05e3b66)

* Adding packaging.deploting and cleaning up stage to the jenkinsfile

  ![image](https://github.com/Mubarokahh/DevOps-Projects/assets/135038657/27779484-fbc3-40e2-86e7-c9bb89db465a)

* The oustcome
![image](https://github.com/Mubarokahh/DevOps-Projects/assets/135038657/cafebeba-71a4-4caa-a683-657f25703322)

## Running Ansible Playbook from jenkins

* Installed ansible.
* Installing Ansible plugin in Jenkins UI to run ansible commands
  
  ![image](https://github.com/Mubarokahh/DevOps-Projects/assets/135038657/ca5bae59-21c6-49c6-ab7c-c6da43da059b)

NOTE: Creating Jenkinsfile from scratch. (Delete all you currently have in there and start all over to get Ansible to run successfully)

* For ansible to be able to locate the roles in my playbook, the ansible.cfg is copied to '/deploy' folder and then exported from the Jenkinsfile code
* Using the jenkins pipeline syntax Ansible tool to generate syntax for executing playbook which runs with tags 'webserver'.

  ![image](https://github.com/Mubarokahh/DevOps-Projects/assets/135038657/e0031a88-c9b2-4885-8e60-52e86cf8f0f6)

  ![image](https://github.com/Mubarokahh/DevOps-Projects/assets/135038657/1ea9e56c-0bd3-4cec-96c0-20e164fbafae)

* Jenkinsfile code for ansible-config-mgt job

 ![image](https://github.com/Mubarokahh/DevOps-Projects/assets/135038657/83b37d6a-5140-4774-af1a-129c08cfc8d3)

 ![image](https://github.com/Mubarokahh/DevOps-Projects/assets/135038657/b2fdc674-859b-4923-b8d3-10d4317e3c51)

* sucess in the blue ocean pipeline on both branches
  
 ![image](https://github.com/Mubarokahh/DevOps-Projects/assets/135038657/ab4182a1-2f90-4745-b0ac-a51bf59e2500)


 ![image](https://github.com/Mubarokahh/DevOps-Projects/assets/135038657/139fe16e-ad83-4948-bbe9-6d3a2e8e87ed)

 ## Parameterizing Jenkinsfile For Ansible Deployment

* Below is the paramter for the the previous pipeline

    `parameters {
      string(name: 'inventory', defaultValue: 'dev.yml',  description: 'This is the inventory file for the environment to deploy configuration')
    }`
![image](https://github.com/Mubarokahh/DevOps-Projects/assets/135038657/0766d379-ac75-4eec-88d9-a38c12dcffde)

* Updating the prameter to "sit.yml"
  The sit.yml file will contain the following.


```
  [tooling]
<SIT-Tooling-Web-Server-Private-IP-Address>

[todo]
<SIT-Todo-Web-Server-Private-IP-Address>

[nginx]
<SIT-Nginx-Private-IP-Address>

[db:vars]
ansible_user=ec2-user
ansible_python_interpreter=/usr/bin/python

[db]
<SIT-DB-Server-Private-IP-Address>

```

## CI/CD PIPELINE FOR TODO APPLICATION

* The goal here is to deploy the application onto servers directly from Artifactory rather than from git.
* Install Jenkins plugins
  1. Plot plugin
  2. Artifactory plugin
 * The plot plugin is used to display tests reports, and code coverage information.
 * The Artifactory plugin is used to easily upload code artifacts into an Artifactory server.

* Congiguring artifactory in jenkins ui

* Setting up artifactory server

   ![image](https://github.com/Mubarokahh/DevOps-Projects/assets/135038657/ae866d46-e50f-421f-9618-18fa3c522a39)


   ![image](https://github.com/Mubarokahh/DevOps-Projects/assets/135038657/f8367d67-ae5a-422a-9b91-3942f3857b9a)

   ![image](https://github.com/Mubarokahh/DevOps-Projects/assets/135038657/7da3fc22-37f9-415d-a7ed-a11a7cd89182)

* Integrate Artifactory repository with Jenkins

  ![image](https://github.com/Mubarokahh/DevOps-Projects/assets/135038657/1e34699a-c5c2-47fb-b0a2-8a245bbce7a1)

  ## Integrate artifactory repository with jenkins

 * Creating a dummy Jenkinsfile in the repository


 * Creating database and user. NOTE: The task of setting the database is done by the MySQL ansible role

   ![image](https://github.com/Mubarokahh/DevOps-Projects/assets/135038657/e2c287e5-058d-434a-993b-529b26541b3e)

 *  Creating Jenkinsfile for the php-todo repository on the main branch

   ![image](https://github.com/Mubarokahh/DevOps-Projects/assets/135038657/b77e9317-a216-47dd-8a75-6bf2cfaa6dbd)


 *  Creating php-todo pipeline job from the Blue Ocean UI
   

   ![image](https://github.com/Mubarokahh/DevOps-Projects/assets/135038657/032ae5b7-b42e-467b-abdb-5b791285934a)

 * After a successful run of the pipeline job, confirming on the database server by running SHOW TABLES command to see the tables being created

   ![image](https://github.com/Mubarokahh/DevOps-Projects/assets/135038657/f0801b0d-99df-4c21-a2cd-068926538075)

## Structuring Jenkins file
* Including unit test stage in the Jenkinsfile

  ![image](https://github.com/Mubarokahh/DevOps-Projects/assets/135038657/c36468a8-328f-4813-aac5-1f4408997ea4)

## Code Quality Analysis

* Adding Code Quality stage with phploc tool and will save the output in build/logs/phploc.csv

 ``` 
 stage('Code Analysis') {
  steps {
        sh 'phploc app/ --log-csv build/logs/phploc.csv'

  }
}
```

![image](https://github.com/Mubarokahh/DevOps-Projects/assets/135038657/558c5fc0-6aba-4e73-8cb1-5dc106ab9a9e)


* Adding the plot code coverage report stage in the Jenkins file

```
   stage('Plot Code Coverage Report') {
      steps {

            plot csvFileName: 'plot-396c4a6b-b573-41e5-85d8-73613b2ffffb.csv', csvSeries: [[displayTableFlag: false, exclusionValues: 'Lines of Code (LOC),Comment Lines of Code (CLOC),Non-Comment Lines of Code (NCLOC),Logical Lines of Code (LLOC)                          ', file: 'build/logs/phploc.csv', inclusionFlag: 'INCLUDE_BY_STRING', url: '']], group: 'phploc', numBuilds: '100', style: 'line', title: 'A - Lines of code', yaxis: 'Lines of Code'
            plot csvFileName: 'plot-396c4a6b-b573-41e5-85d8-73613b2ffffb.csv', csvSeries: [[displayTableFlag: false, exclusionValues: 'Directories,Files,Namespaces', file: 'build/logs/phploc.csv', inclusionFlag: 'INCLUDE_BY_STRING', url: '']], group: 'phploc', numBuilds: '100', style: 'line', title: 'B - Structures Containers', yaxis: 'Count'
            plot csvFileName: 'plot-396c4a6b-b573-41e5-85d8-73613b2ffffb.csv', csvSeries: [[displayTableFlag: false, exclusionValues: 'Average Class Length (LLOC),Average Method Length (LLOC),Average Function Length (LLOC)', file: 'build/logs/phploc.csv', inclusionFlag: 'INCLUDE_BY_STRING', url: '']], group: 'phploc', numBuilds: '100', style: 'line', title: 'C - Average Length', yaxis: 'Average Lines of Code'
            plot csvFileName: 'plot-396c4a6b-b573-41e5-85d8-73613b2ffffb.csv', csvSeries: [[displayTableFlag: false, exclusionValues: 'Cyclomatic Complexity / Lines of Code,Cyclomatic Complexity / Number of Methods ', file: 'build/logs/phploc.csv', inclusionFlag: 'INCLUDE_BY_STRING', url: '']], group: 'phploc', numBuilds: '100', style: 'line', title: 'D - Relative Cyclomatic Complexity', yaxis: 'Cyclomatic Complexity by Structure'      
            plot csvFileName: 'plot-396c4a6b-b573-41e5-85d8-73613b2ffffb.csv', csvSeries: [[displayTableFlag: false, exclusionValues: 'Classes,Abstract Classes,Concrete Classes', file: 'build/logs/phploc.csv', inclusionFlag: 'INCLUDE_BY_STRING', url: '']], group: 'phploc', numBuilds: '100', style: 'line', title: 'E - Types of Classes', yaxis: 'Count'
            plot csvFileName: 'plot-396c4a6b-b573-41e5-85d8-73613b2ffffb.csv', csvSeries: [[displayTableFlag: false, exclusionValues: 'Methods,Non-Static Methods,Static Methods,Public Methods,Non-Public Methods', file: 'build/logs/phploc.csv', inclusionFlag: 'INCLUDE_BY_STRING', url: '']], group: 'phploc', numBuilds: '100', style: 'line', title: 'F - Types of Methods', yaxis: 'Count'
            plot csvFileName: 'plot-396c4a6b-b573-41e5-85d8-73613b2ffffb.csv', csvSeries: [[displayTableFlag: false, exclusionValues: 'Constants,Global Constants,Class Constants', file: 'build/logs/phploc.csv', inclusionFlag: 'INCLUDE_BY_STRING', url: '']], group: 'phploc', numBuilds: '100', style: 'line', title: 'G - Types of Constants', yaxis: 'Count'
            plot csvFileName: 'plot-396c4a6b-b573-41e5-85d8-73613b2ffffb.csv', csvSeries: [[displayTableFlag: false, exclusionValues: 'Test Classes,Test Methods', file: 'build/logs/phploc.csv', inclusionFlag: 'INCLUDE_BY_STRING', url: '']], group: 'phploc', numBuilds: '100', style: 'line', title: 'I - Testing', yaxis: 'Count'
            plot csvFileName: 'plot-396c4a6b-b573-41e5-85d8-73613b2ffffb.csv', csvSeries: [[displayTableFlag: false, exclusionValues: 'Logical Lines of Code (LLOC),Classes Length (LLOC),Functions Length (LLOC),LLOC outside functions or classes ', file: 'build/logs/phploc.csv', inclusionFlag: 'INCLUDE_BY_STRING', url: '']], group: 'phploc', numBuilds: '100', style: 'line', title: 'AB - Code Structure by Logical Lines of Code', yaxis: 'Logical Lines of Code'
            plot csvFileName: 'plot-396c4a6b-b573-41e5-85d8-73613b2ffffb.csv', csvSeries: [[displayTableFlag: false, exclusionValues: 'Functions,Named Functions,Anonymous Functions', file: 'build/logs/phploc.csv', inclusionFlag: 'INCLUDE_BY_STRING', url: '']], group: 'phploc', numBuilds: '100', style: 'line', title: 'H - Types of Functions', yaxis: 'Count'
            plot csvFileName: 'plot-396c4a6b-b573-41e5-85d8-73613b2ffffb.csv', csvSeries: [[displayTableFlag: false, exclusionValues: 'Interfaces,Traits,Classes,Methods,Functions,Constants', file: 'build/logs/phploc.csv', inclusionFlag: 'INCLUDE_BY_STRING', url: '']], group: 'phploc', numBuilds: '100', style: 'line', title: 'BB - Structure Objects', yaxis: 'Count'

      }
    }
```
![image](https://github.com/Mubarokahh/DevOps-Projects/assets/135038657/3e8c75ac-5149-42c1-a160-caa1e15ed026)

* Adding the package artifacts stage which archives the application code

```
    stage('Package Artifact') {
      steps {
            sh 'zip -qr php-todo.zip ${WORKSPACE}/*'
      }
    }
  ```
* Uploading the artifacts to the Artifactory repository in this stage:

```
 }
    stage ('Upload Artifact to Artifactory') {
          steps {
            script { 
                 def server = Artifactory.server 'artifactory-server'                 
                 def uploadSpec = """{
                    "files": [
                      {
                       "pattern": "php-todo.zip",
                       "target": "wurapbl/php-todo",
                       "props": "type=zip;status=ready"

                       }
                    ]
                 }""" 

                 server.upload spec: uploadSpec
               }
            }
```
  ![image](https://github.com/Mubarokahh/DevOps-Projects/assets/135038657/6463df72-de2e-4cd6-a5e4-c91efba1c2ef)

  * Deploying the application to the dev environment by launching Ansible pipeline job(ansible-config-mgt)

```
    stage ('Deploy to Dev Environment') {
    steps {
    build job: 'ansible-config-mgt/main', parameters: [[$class: 'StringParameterValue', name: 'env', value: 'dev']], propagate: false, wait: true
    }
  }
```

![image](https://github.com/Mubarokahh/DevOps-Projects/assets/135038657/6bbdfea4-7b87-4d7a-938c-cd352da7a2fa)


## Setting Up The SonarQube Server

SonarQube server is set up and configured in this way to guarantee that only code meeting the necessary code coverage requirements and other quality standards reaches the development environment. In this project predefined Quality Gates (also known as The Sonar Way) will be used.

* On the SonarQube server, performing the following command on the terminal which makes the session changes persist beyond the current session terminal to ensure optimal performance of the tool
  
```
 sudo sysctl -w vm.max_map_count=262144
 sudo sysctl -w fs.file-max=65536
 ulimit -n 65536
 ulimit -u 4096

```      
 * To make a permanent change, edit the file /etc/security/limits.conf and append the below

```
   sonarqube   -   nofile   65536
   sonarqube   -   nproc    4096
```

![image](https://github.com/Mubarokahh/DevOps-Projects/assets/135038657/eca5f18e-b60a-41b4-881b-9832a9015f68)

* Updating and upgrading the server:
  
  ```
  sudo apt update
  sudo apt upgrade
  
  ```
* Install OpenJDK and Java Runtime Environment (JRE) 11

  ```
  sudo apt-get install openjdk-11-jdk -y
  sudo apt-get install openjdk-11-jre -y
  
  ```
  * Verifying the java version that is in use
  
     `java --version`
    
   ![image](https://github.com/Mubarokahh/DevOps-Projects/assets/135038657/b2871de5-3f56-4caf-b57b-b3c14cd683cb)

* To install postgresql database, adding the postgresql repo to the repo list

  `sudo sh -c 'echo "deb http://apt.postgresql.org/pub/repos/apt/ `lsb_release -cs`-pgdg main" >> /etc/apt/sources.list.d/pgdg.list'`

* Downloading Postgresql software key:

  `wget -q https://www.postgresql.org/media/keys/ACCC4CF8.asc -O - | sudo apt-key add -`

* Installing postgresql database server:

   `sudo apt-get -y install postgresql postgresql-contrib`

* Starting postgresql server:

  `sudo systemctl start postgresql`
  
* Enabling it to start automatically:

  `sudo systemctl enable postgresql`
  
  ![image](https://github.com/Mubarokahh/DevOps-Projects/assets/135038657/9211aaec-710c-4c67-a54c-cac525032be8)


* Changing the password for the default postgres user:

`sudo passwd postgres`

* Switching to the postgres user:
  
  ` su – postgres`
  
* Creating a new user ‘sonar’:
  
 ` createuser sonar`
 
* Activating postgresql shell:
  
  `psql`
  
* Setting password for the newly created user for the SonarQube databases:
  
 `ALTER USER sonar WITH ENCRYPTED password 'sonar';`
 
* Creating a new database for Postgresql database:

  `CREATE DATABASE sonarqube OWNER sonar;`
  
* Granting all privileges to the user sonar on sonarqube database:

   `grant all privileges on DATABASE sonarqube to sonar;`
  
* Exiting from the shell

  ![image](https://github.com/Mubarokahh/DevOps-Projects/assets/135038657/0fa617ff-bbed-470b-a935-982c00b35c00)

* To install SonarQube software, navigating to the '/tmp' folder to temporarily download the installation files:

 ` cd /tmp && sudo wget https://binaries.sonarsource.com/Distribution/sonarqube/sonarqube-7.9.3.zip`

 * Unzip the archive setup to /opt directory

`sudo unzip sonarqube-7.9.3.zip -d /opt`

 * Move extracted setup to /opt/sonarqube directory

 `sudo mv /opt/sonarqube-7.9.3 /opt/sonarqube`

 ## NOTE
  Sonarqube cannot be run as a root user, if it is being run has a root user, it will authomatically stop.The ideal approach will be to create a separate group and a user to run SonarQube

 * Creating a group sonar

  `sudo groupadd sonar`

 * Adding a user with control over the /opt/sonarqube directory

 `sudo useradd -c "user to run SonarQube" -d /opt/sonarqube -g sonar sonar 
 sudo chown sonar:sonar /opt/sonarqube -R`
 
  * Editing SonarQube configuration file
    
  `sudo vim /opt/sonarqube/conf/sonar.properties`
  
* Uncomment the following lines  and provide the values of PostgreSQL Database username and password:


#sonar.jdbc.username=
#sonar.jdbc.password=

* Editing the sonar script file and set RUN_AS_USER:

   `sudo nano /opt/sonarqube/bin/linux-x86-64/sonar.sh`

  ![image](https://github.com/Mubarokahh/DevOps-Projects/assets/135038657/a27c1a38-0416-4c9f-986d-1d321ed03c55)

## Starting Sonarqube

* Switch to sonar user

`sudo su sonar`

* Move to the script directory

`cd /opt/sonarqube/bin/linux-x86-64/`

* Running the script to start SonarQube

`./sonar.sh start`

* Checking SonarQube running status:

./sonar.sh status

![image](https://github.com/Mubarokahh/DevOps-Projects/assets/135038657/3f3a9dc1-4ece-4937-8dde-16818082293b)


* To check SonarQube logs:

   `tail /opt/sonarqube/logs/sonar.log`

*To Configure SonarQube as a service, the SonarQube server is stopped first:

`cd /opt/sonarqube/bin/linux-x86-64/`

* Stopping the server:

  `./sonar.sh stop`

## Configure SonarQube to run as a systemd service

* Stop the currently running SonarQube service

 `cd /opt/sonarqube/bin/linux-x86-64/`
 
* Run the script to start SonarQube

`./sonar.sh stop`

* Create a systemd service file for SonarQube to run as System Startup.

 `sudo nano /etc/systemd/system/sonar.service`

 ![image](https://github.com/Mubarokahh/DevOps-Projects/assets/135038657/9237001d-2869-497f-8cde-071517f8e66b)

* Starting the SonarQube service and enabling it:
  
 `sudo systemctl start sonar`

 `sudo systemctl enable sonar`

 `sudo systemctl status sonar`

 * Opening TCP port 9000 on the security group

 * Accessing SonarQube through the browser by entering the SonarQube server’s IP address followed by port 9000: http://<server's_IP_adress>:9000

## Configure Sonarqube And Jenkins For Quality Gate

* Generating authentication token in SonarQube server by navigating from 'My Account' to 'security':




* Configuring Quality Gate Jenkins Webhook in SonarQube by navigating from 'Administration' to 'Configuration' to 'webhook' and 'create' and then specifying the URL as this: http://<Jenkins ip address>/sonarqube-webhook/

* Installing SonarScanner plugin in jenkins

  ![image](https://github.com/Mubarokahh/DevOps-Projects/assets/135038657/9f63c572-d282-44cf-9e45-f91b5a6e63b7)
  
* Navigating to 'Configure System' in Jenkins to add SonarQube server details with the generated token

* Setting the SonarQube scanner by navigating from 'manage jenkins' to 'Global Tool Configuration'

* Configuring the sonar-scanner.properties file in which SonarQube will require to function during pipeline execution:

  `cd /var/lib/jenkins/tools/hudson.plugins.sonar.SonarRunnerInstallation/SonarQubeScanner/conf/`
  
* Editing and entering the following configuration: $ sudo vi sonar-scanner.properties













  








  



 






 











## Error Encountered during the implementation of this project


## Error Code

* Whilst running the pipeline, the 

## Error Execution





  
















