
 # DEVOPS TOOLING WEBSITE SOLUTION

  In this project, a set of Devops tools will be introduced to help the team in a day to day in managing,developing,testing, deploying and monitoring diffferent projects.
  The tools that will be used are well known tools and widely used by Devops teams, a single Devops toolind solution that will consist of the following:

*  Jenkins – free and open source automation server used to build CI/CD pipelines.
  
*  Kubernetes – an open-source container-orchestration system for automating computer application deployment, scaling, and management.

*  Jfrog Artifactory – Universal Repository Manager supporting all major packaging formats, build tools and CI servers. Artifactory.

*  Rancher – an open source software platform that enables organizations to run and manage Docker and Kubernetes in production.
Grafana – a multi-platform open source analytics and interactive visualization web application.

*  Prometheus – An open-source monitoring system with a dimensional data model, flexible query language, efficient time series database and modern alerting approach.

*  Kibana – Kibana is a free and open user interface that lets you visualize your Elasticsearch data and navigate the Elastic Stack.

  ## Setup and technologies used in this Project

  The aim of this project is implementation of a tooling website solution which gives acess to Devops tools within the corporate infracturucture easily accessible.
  A solution that consists of the folowing will be implemented:

 *  Infrastructure: AWS
  
 *  Webserver Linux: Red Hat Enterprise Linux 8

 *  Database Server: Ubuntu 20.04 + MySQL

 *  Storage Server: Red Hat Enterprise Linux 8 + NFS Server

 *  Programming Language: PHP

 *  Code Repository: GitHub

  ## Setting up the NFS server

  I spinned up an EC2 instance with RHEL Linux 8 Operating System  that will be used as the NFS server, Once the server started running, i created 3 volumes and attached them to the NFS server.

  ![image](https://github.com/Mubarokahh/DevOps-Projects/assets/135038657/2d343f54-58af-42ce-bc97-684dd04b0ee0)
