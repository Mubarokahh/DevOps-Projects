# MERN Stack Deployment To AWS
In this project, i will be implememting a simple book regular web form using MERN stack.
This technology stack consists of:
* MongoDB (A document database that allows storage and retriever of data).
* Express {Back-end application framework}
* Angular {Front-end application framework}
* NodeJS {A javascript run time environment}

## Step 1: Setting up a virtual environment in AWS Cloud
I set up an EC2 instance in AWS.I then open my mac terminal and ran the SSH comands in order to connect to the server I have running in AWS cloud using my private key.Upon connecting to the virtual environment, i configured the virtual server (Ubuntu) that is now running on my mac terminal using the following commands:
1. sudo apt update
2. suso apt upgrade

## Step 2: Installing NodeJS on the Server
 To install NodeJS,add the certificates below:
 sudo apt -y install curl dirmngr apt-transport-https lsb-release ca-certificates

curl -sL https://deb.nodesource.com/setup_18.x | sudo -E bash - 

 Run this command afterwards: sudo apt install -y nodejs
  This command installs both NodeJS and NPM (Node Package Manager)

 ## Step 3: Installing Mongodb
 * Import the public key used by the package management syatem by running: sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv 0C49F3730359A14518585931BC711F9BA15703C6
 * Creating the list file for this version of ubuntu: echo "deb [ arch=amd64 ] https://repo.mongodb.org/apt/ubuntu trusty/mongodb-org/3.4 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-3.4.list

 * Install MongoDB: sudo apt install -mongodb
 * Starting the server: sudo systemctl start mongod
 * Verifying if the server is running successfully: sudo systemctl status mongod

 ![image.png]

 * install npm-Node package manager: sudo apt install -y npm

 ## Step 4: Instll Body-parser Package
  * To install this package: sudo npm install body-parser

  * Create a folder named Books: mkdir Books && cd Books
  * Initialize npm project in the book directory: npm init
  * Creating a file in the books directory  named server.js and opening it with : vi server
  * Paste the web server code below into the file and save:
  var express = require('express');
var bodyParser = require('body-parser');
var app = express();
app.use(express.static(__dirname + '/public'));
app.use(bodyParser.json());
require('./apps/routes')(app);
app.set('port', 3300);
app.listen(app.get('port'), function() {
    console.log('Server up: http://localhost:' + app.get('port'));
});

## STEP 5: Installing Express and setting up routes to the server
* Installing express and mongoose to establish a schema for the database to store data of our book register: $ sudo npm install express mongoose
* Creating a folder named apps in book directory: mkdir apps & cd apps
* create a file in the apps directory named routes.js: touch routes.js
* open the file and paste the following code:
const Book = require('./models/book');

module.exports = function(app) {
  app.get('/book', function(req, res) {
    Book.find({}).then(result => {
      res.json(result);
    }).catch(err => {
      console.error(err);
      res.status(500).send('An error occurred while retrieving books');
    });
  });

  app.post('/book', function(req, res) {
    const book = new Book({
      name: req.body.name,
      isbn: req.body.isbn,
      author: req.body.author,
      pages: req.body.pages
    });
    book.save().then(result => {
      res.json({
        message: "Successfully added book",
        book: result
      });
    }).catch(err => {
      console.error(err);
      res.status(500).send('An error occurred while saving the book');
    });
  });

  app.delete("/book/:isbn", function(req, res) {
    Book.findOneAndRemove(req.query).then(result => {
      res.json({
        message: "Successfully deleted the book",
        book: result
      });
    }).catch(err => {
      console.error(err);
      res.status(500).send('An error occurred while deleting the book');
    });
  });

  const path = require('path');
  app.get('*', function(req, res) {
    res.sendFile(path.join(__dirname, 'public', 'index.html'));
  });
};
  * Create a folder in apps named models and enter the directory: mkdir models && cd models
  * create a file in the models folder named book.js
  * open the book.js with file editor :vi book.js and paste the code below,
  var mongoose = require('mongoose');
var dbHost = 'mongodb://localhost:27017/test';
mongoose.connect(dbHost);
mongoose.connection;
mongoose.set('debug', true);
var bookSchema = mongoose.Schema( {
  name: String,
  isbn: {type: String, index: true},
  author: String,
  pages: Number
});
var Book = mongoose.model('Book', bookSchema);
module.exports = mongoose.model('Book', bookSchema);

## STEP 6; Accessing the Routes With AngularJS
* Chnage directory back to books 
* create a folder named public and enter that directory: mkdir public && cd public
* create a file named script.js and open with file editor: vi script.js
* paste the following code in the file:
Copy and paste the Code below (controller configuration defined) into the script.js file.

var app = angular.module('myApp', []);
app.controller('myCtrl', function($scope, $http) {
  $http( {
    method: 'GET',
    url: '/book'
  }).then(function successCallback(response) {
    $scope.books = response.data;
  }, function errorCallback(response) {
    console.log('Error: ' + response);
  });
  $scope.del_book = function(book) {
    $http( {
      method: 'DELETE',
      url: '/book/:isbn',
      params: {'isbn': book.isbn}
    }).then(function successCallback(response) {
      console.log(response);
    }, function errorCallback(response) {
      console.log('Error: ' + response);
    });
  };
  $scope.add_book = function() {
    var body = '{ "name": "' + $scope.Name + 
    '", "isbn": "' + $scope.Isbn +
    '", "author": "' + $scope.Author + 
    '", "pages": "' + $scope.Pages + '" }';
    $http({
      method: 'POST',
      url: '/book',
      data: body
    }).then(function successCallback(response) {
      console.log(response);
    }, function errorCallback(response) {
      console.log('Error: ' + response);
    });
  };
});
 * Create a file named "index.html" in the public folder and open with file editor: vi index.html
 * Paste the following code and save: 
 <!doctype html>
<html ng-app="myApp" ng-controller="myCtrl">
  <head>
    <script src="https://ajax.googleapis.com/ajax/libs/angularjs/1.6.4/angular.min.js"></script>
    <script src="script.js"></script>
  </head>
  <body>
    <div>
      <table>
        <tr>
          <td>Name:</td>
          <td><input type="text" ng-model="Name"></td>
        </tr>
        <tr>
          <td>Isbn:</td>
          <td><input type="text" ng-model="Isbn"></td>
        </tr>
        <tr>
          <td>Author:</td>
          <td><input type="text" ng-model="Author"></td>
        </tr>
        <tr>
          <td>Pages:</td>
          <td><input type="number" ng-model="Pages"></td>
        </tr>
      </table>
      <button ng-click="add_book()">Add</button>
    </div>
    <hr>
    <div>
      <table>
        <tr>
          <th>Name</th>
          <th>Isbn</th>
          <th>Author</th>
          <th>Pages</th>

        </tr>
        <tr ng-repeat="book in books">
          <td>{{book.name}}</td>
          <td>{{book.isbn}}</td>
          <td>{{book.author}}</td>
          <td>{{book.pages}}</td>

          <td><input type="button" value="Delete" data-ng-click="del_book(book)"></td>
        </tr>
      </table>
    </div>
  </body>
</html>

* Run the following command to start the server : node server.js

## Step 7: Updating the EC2 Instance Security Group
 * Configuring the security group of the EC2 instance to be able to listen to port 3300

## Last Step
* Opening up my browser and entering my public address with the port number 3300 to access the Book register application:

