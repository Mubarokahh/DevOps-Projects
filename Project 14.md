# Simulating A Typical CI/CD Pipeline For A PHP Based Application

The CI/CD concept is applied in this project by pushing a PHP application from Github to Jenkins to perform a multi-branch pipeline job (build job is done on each branches of a repository simultaneously) . To ensure continuous integration of codes from many developers, this is done. Following that, the build job's artifacts are packaged and sent to a sonarqube server for testing before being deployed to an artifactory, where ansible is used to trigger a job that will deploy the application to a production environment.The CI/CD pipeline is represented by the diagram below.

![image](https://github.com/Mubarokahh/DevOps-Projects/assets/135038657/84540039-1393-49e3-855a-4381e0bca799)
