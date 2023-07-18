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
