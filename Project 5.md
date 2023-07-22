## CLIENT-SERVER ARCHITECTURE WITH MYSQL

A client-server is an architecture in which two or more computers are connected together over a network and recerive requests between one another.Each machine has its won role in their communication."The Client'is the machine sending the request.'The Server'is the machine responding.

A diagramatic representation of Web Client-Server archotecture is displayed below.


![Client-server](https://github.com/Mubarokahh/DevOps-Projects/assets/135038657/c38dc12f-324d-4f2f-9da0-691dd2bf3746)

According to the example above, a machine trying to access a web site via web server or "curl" command is a CLIENT, and it sends HTTP requests to web server like Apache, Nginx, IIS e.t.c.

Extending this concept further by addind a Database server to the our architecture will lead to the example below


![Client-server2](https://github.com/Mubarokahh/DevOps-Projects/assets/135038657/bbfa886c-ef2f-4913-b8b0-f188a8259f07)


In this case, our Web Server has a role of a “Client” that connects and reads/writes to/from a Database (DB) Server (MySQL, MongoDB, Oracle, SQL Server or any other), and the communication between them happens over a Local Network (it can also be Internet connection, but it is a common practice to place Web Server and DB Server close to each other in local network).

The setup on the diagram above is a typical generic Web Stack architecture that you have already implemented in previous projects (LAMP, LEMP, MEAN, MERN), this architecture can be implemented with many other technologies – various Web and DB servers, from small Single-page applications SPA to large and complex portals.

## IMPLEMENT A CLIENT SERVER ARCHITECTURE USING MYSQL DATABASE MANAGEMENT SYSTEM (DBMS)

To demostretae a basic client-server using MySQL Relational Database Management System (RDBMS). Start by creating two linux-based virtual servers (EC2 instances in AWS)

Server A name - `mysql server`
Server B name - `mysql client`

![image](https://github.com/Mubarokahh/DevOps-Projects/assets/135038657/db3e4a67-2423-4208-8a8c-c1bcc08eb10f)

On mysql server Linux Server install MySQL Server software
`sudo install mysql-server -y`

On mysql client Linux Server install MySQL Client software
`sudo install mysql-client -y`

![image](https://github.com/Mubarokahh/DevOps-Projects/assets/135038657/19dcd567-a71b-4db8-9cad-3bb49dd9dc0e)
MySQL Server

![image](https://github.com/Mubarokahh/DevOps-Projects/assets/135038657/08049c84-eb85-44b9-bf4a-3329039cbb32)
MySQL Client

On mysql client Linux Server install MySQL Client software.
By default, both of your EC2 virtual servers are located in the same local virtual network, so they can communicate to each other using local IP addresses. Use mysql server's local IP address to connect from mysql client. MySQL server uses TCP port 3306 by default, so you will have to open it by creating a new entry in ‘Inbound rules’ in ‘mysql server’ Security Groups. For extra security, do not allow all IP addresses to reach your ‘mysql server’ – allow access only to the specific local IP address of your ‘mysql client’.

![image](https://github.com/Mubarokahh/DevOps-Projects/assets/135038657/9a528385-fa92-4532-8d1c-3043bea4f2e6)

At this point,configure MySQL server to allow connections from remote hosts.
`sudo vi /etc/mysql/mysql.conf.d/mysqld.cnf`

Replace `127.0.0.1`of the bind address to `‘0.0.0.0`:
![image](https://github.com/Mubarokahh/DevOps-Projects/assets/135038657/a921cb6b-18af-4c26-8cfd-9cf138deea7d)

Create a user,database and  grant all privileges to the user on mysql server

Activate mysql shell: 
`sudo mysql`

`CREATE DATABASE whiz_db;`

Creating a remote user with mysql client  ip_address: 
`CREATE USER 'Mubz'@'3.84.57.79' IDENTIFIED BY 'password';`

Granting the remote user full access to the database:
`GRANT ALL PRIVILEGES ON whiz_db.* TO 'Mubz'@'3.84.57.79';`

Lastly, flushing the privileges so that MySQL will begin to use them:
`FLUSH PRIVILEGES;`

Exit mysql;
`exit`

![image](https://github.com/Mubarokahh/DevOps-Projects/assets/135038657/fd011a6e-069f-433f-a54f-16d72e735449)

From mysql client Linux Server connect remotely to mysql server Database Engine without using SSH. You must use the mysql utility to perform this action

![image](https://github.com/Mubarokahh/DevOps-Projects/assets/135038657/bc7d1868-2d0a-41a3-9939-ad60a85f31e4)

Check that you have successfully connected to a remote MySQL server and can perform SQL queries, run the command

`Show databases;`

If you see an output similar to the below image, then you have successfully completed this project – you have deloyed a fully functional MySQL Client-Server set up.

![image](https://github.com/Mubarokahh/DevOps-Projects/assets/135038657/85f57fbb-fd0c-4042-97a0-e9084ec2d5f3)

Congratulations,connection estalished!!!!













