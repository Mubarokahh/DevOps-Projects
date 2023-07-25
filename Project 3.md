# Simple To-Do Application on MERN Web Stack
This project aims to implement web solution based on MERN technology stack in the AWS stack. 

## MERN = MongoDB + ExpressJS + ReactJS + Node.js

MongoDB: A document-based, No-SQL database used to store application data in a form of documents ExpressJS: A server side Web Application framework for Node.js ReactJS: A frontend framework developed by Facebook. It is based on JavaScript, used to build User Interface (UI) components. Node.js: A JavaScript runtime environment. It is used to run JavaScript on a machine rather than in a browser.

![image](https://github.com/Mubarokahh/DevOps-Projects/assets/135038657/038d29eb-77f4-4ffe-ba88-903244476506)

A user interacts with the ReactJS UI components at the application front-end, which is located in the browser, as demonstrated in the above picture. Through ExpressJS running on top of NodeJS, the application backend located on a server serves this frontend. Any interaction that results in a request for a data modification is sent to the Express server, which is based on NodeJS. Express pulls data from the MongoDB database as needed and delivers it to the frontend of the application, where it is then shown to the user.


## Step 1: Setting up a virtual environment in AWS Cloud
I set up an EC2 instance in AWS.I then open my mac terminal and ran the SSH comands in order to connect to the server I have running in AWS cloud using my private key.Upon connecting to the virtual environment, i configured the virtual server (Ubuntu) that is now running on my mac terminal using the following commands:
1. `sudo apt update`
2. `suso apt upgrade`

![image](https://github.com/Mubarokahh/DevOps-Projects/assets/135038657/6a80e081-f961-4f33-b84b-d8bb501b0b24)

  To unveil the location of NodeJS SOFTWARE FROM Ubuntu repositories
 ` curl -fsSL https://deb.nodesource.com/setup_18.x | sudo -E bash -`

  ## Step 2: Installing NodeJS on the Server
  
 * To install NodeJS, run: 

`sudo apt-get install -y nodejs`

  This command installs both NodeJS and NPM (Node Package Manager)

  ![image](https://github.com/Mubarokahh/DevOps-Projects/assets/135038657/d5e54d63-89a9-41b2-a2da-bc9811a3aba6)


  ## STEP 3:Application Code Setup
  * Creating a new directory:

     ` mkdir Todo`
  * To verify that the directory is created:

    `ls`
  * Changing the current directory to work in Todo directory:
    
 `cd Todo`
  * Initializing my project which will create package.json with this command:

      `npm init`
    
  * Following the prompt and accept to write out the package.json file

    ![image](https://github.com/Mubarokahh/DevOps-Projects/assets/135038657/49c9efa3-e275-46d9-9b14-b734c0e1102f)


  ## STEP 4:Installing ExpressJS
  * To use express, install it via npm :

    ` npm install express`
  * Creating a new file index.js:

    ` touch index.js`
  * Installing the dotenv module:

    ` npm install dotenv`

   ![image](https://github.com/Mubarokahh/DevOps-Projects/assets/135038657/ff6d7775-48ab-4fcd-be92-c51063d36d6a)


  * To edit the index.js file creted:

    `vim index.js`
    
  * Enter the following script into the file
    
` const express = require('express');
require('dotenv').config();

const app = express();

const port = process.env.PORT || 5000;

app.use((req, res, next) => {
res.header("Access-Control-Allow-Origin", "\*");
res.header("Access-Control-Allow-Headers", "Origin, X-Requested-With, Content-Type, Accept");
next();
});

app.use((req, res, next) => {
res.send('Welcome to Express');
});

app.listen(port, () => {
console.log(`Server running on port  ${port}`)
});
`


SIDENOTE: The server has been specified to run on code 5000 in the script above.

* To start and check if the server works:node index.js

![image](https://github.com/Mubarokahh/DevOps-Projects/assets/135038657/2a5d0bce-2fc6-40d0-8817-0c998cdce221)


* Configuring the EC2 security group,creating an inbound rule to open port 5000

![image](https://github.com/Mubarokahh/DevOps-Projects/assets/135038657/82c6124a-a066-43fd-beb1-701b173c1932)


## STEP 5: Setting up Routes
 Configuring routes to handle these three actions for the To-Do application.

 1. Create a new task
 2. Display list of all tasks
 3. Delete a completed task

 * Creating a folder for our routes: `mkdir routes`
 * Changing directory to routes: ` cd routes`
 * Creating a file called api.js: ` touch api.js`
 * Opening the file with the command:` vim api.js`
   
 * Entering the following script to the routes folder.

` const express = require ('express');
const router = express.Router();

router.get('/todos', (req, res, next) => {

});

router.post('/todos', (req, res, next) => {

});

router.delete('/todos/:id', (req, res, next) => {

})

module.exports = router;
`
## STEP 6:Setting up a Model
Firstly, install a Nodejs package called mongoose which makes working with mongodb easier. 
*  Change directory back to TO-DO folder; cd todo
*  Installing Mongoose: npm install mongoose
*  Creating a new folder called models: $ mkdir models
*  Changing the directory to models: $ cd models
*  Creating a new file called todo.js: $ touch todo.js
*  Open the todo.js file with vim todo.js command and enter the        following code

`const mongoose = require('mongoose');
const Schema = mongoose.Schema;

//create schema for todo
const TodoSchema = new Schema({
action: {
type: String,
required: [true, 'The todo text field is required']
}
})
//create model for todo
const Todo = mongoose.model('todo', TodoSchema);

module.exports = Todo;`

* Updating the routes file in the routes directory in order to work with the new model created and entering the code:

    `const express = require ('express');
    const router = express.Router();
    const Todo = require('../models/todo');

    router.get('/todos', (req, res, next) => {`

    //this will return all the data, exposing only the id and action field to the client
    
    Todo.find({}, 'action')
    .then(data => res.json(data))
    .catch(next)
    });

    router.post('/todos', (req, res, next) => {
    if(req.body.action){
    Todo.create(req.body)
    .then(data => res.json(data))
    .catch(next)
    }else {
    res.json({
    error: "The input field is empty"
    })
    }
    });

    router.delete('/todos/:id', (req, res, next) => {
    Todo.findOneAndDelete({"_id": req.params.id})
    .then(data => res.json(data))
    .catch(next)
    })

    module.exports = router;`

    ## STEP 7: Launching MongoDG Database using mLab

    We need a database where we will store our data. For this we will make use of mLab. mLab provides MongoDB database as a service solution (DBaaS), so to make life easy, you will need to sign up for a shared clusters free account, which is ideal for our use case.Follow the sign up process, select AWS as the cloud provider, and choose a region near you.
  
  ![image](https://github.com/Mubarokahh/DevOps-Projects/assets/135038657/4ee70c0e-7965-41a0-9f9a-90540f872364)

    The mLab provides MongoDB database as a service solution (DBaaS). To do this
    * Create a mLab account 
    * Set up  a MongoDB database and collection inside mLab
    * Create a .env file in Todo directory: touch .env
    * Open the file: $ vi .env
    * Add the connection string to access the database embeded in it
      
     DB = 'mongodb+srv://<username>:<password>@<network-address>/<dbname>?retryWrites=true&w=majority' 
     SIDENOTE: replace the username, password and network-address, database name with the credentials used in creating the mongodb database

  
     Updating the index.js file to reflect the use of .env so that Node.js can connect to the database. Simply delete existing content in the file, and update it with the entire code below. To do that using vim, follow below steps
  
     * Open the file with: `vim index.js`
     * Delete the current script with: :` %d`
     * Paste the following code:
       
     `const express = require('express');
const bodyParser = require('body-parser');
const mongoose = require('mongoose');
const routes = require('./routes/api');
const path = require('path');
require('dotenv').config();

const app = express();

const port = process.env.PORT || 5000;

//connect to the database
mongoose.connect(process.env.DB, { useNewUrlParser: true, useUnifiedTopology: true })
.then(() => console.log(`Database connected successfully`))
.catch(err => console.log(err));

//since mongoose promise is depreciated, we overide it with node's promise
mongoose.Promise = global.Promise;

app.use((req, res, next) => {
res.header("Access-Control-Allow-Origin", "\*");
res.header("Access-Control-Allow-Headers", "Origin, X-Requested-With, Content-Type, Accept");
next();
});

app.use(bodyParser.json());

app.use('/api', routes);

app.use((err, req, res, next) => {
console.log(err);
next();
});

app.listen(port, () => {
console.log(`Server running on port ${port}`)
});`

 * Start the derver with:
  ` node index.js`

Note: I was not able to connect to the database via port 5000 because it was already running on the system, so i had to update the script of index.js from Port 5000 to 5001. I also had to add a new inbound rule running at port 5001 to the EC2 server.

![image](https://github.com/Mubarokahh/DevOps-Projects/assets/135038657/6c87b42b-739c-49ca-bfe5-85dda7251178)

You shall see a message ‘Database connected successfully’, if so – we have our backend configured. Now we are going to test it.

## Testing Backend Code without Frontend using RESTful API


So far we have written backend part of our To-Do application, and configured a database, but we do not have a frontend UI yet. We need ReactJS code to achieve that. But during development, we will need a way to test our code using RESTfull API. Therefore, we will need to make use of some API development client to test our code.

In this project, we will use Postman to test the API. You should test all the API endpoints and make sure they are working. For the endpoints that require body, you should send JSON back with the necessary fields since it’s what we setup in our code.
Now open your Postman, create a POST request to the API http://<PublicIP-or-PublicDNS>:5000/api/todos. This request sends a new task to our To-Do list so the application could store it in the database.
Note: make sure your set header key Content-Type as application/json
![image](https://github.com/Mubarokahh/DevOps-Projects/assets/135038657/02f764c8-b30e-48e8-9801-e18843643cd5)

Create a GET request to your API on http://<PublicIP-or-PublicDNS>:5000/api/todos. This request retrieves all existing records from out To-do application (backend requests these records from the database and sends it us back as a response to GET request).

![image](https://github.com/Mubarokahh/DevOps-Projects/assets/135038657/29c60ce9-8750-4f16-bb24-20b307b4d8fc)

## FRONT-END CREATION
Since we are done with the functionality we want from our backend and API, it is time to create a user interface for a Web client (browser) to interact with the application via API. To start out with the frontend of the To-do app, we will use the create-react-app command to scaffold our app.
* In the same root directory as your backend code, which is the Todo directory, run:

 ` npx create-react-app client`
 This will create a new folder in your Todo directory called client, where you will add all the react code.

 ![image](https://github.com/Mubarokahh/DevOps-Projects/assets/135038657/3ac3c975-3a3d-463e-8150-ab644f5e2732)

 ## Running React App

 Before testing the react app, there are some dependencies that need to be installed.
 * Install concurrently. It is used to run more than one command simultaneously from the same terminal window.

  `npm install concurrently --save-dev`

  ![image](https://github.com/Mubarokahh/DevOps-Projects/assets/135038657/9e38ae57-bc2b-4b1e-88e8-f8deae970960)

 * Install nodemon. It is used to run and monitor the server. If there is any change in the server code, nodemon will restart it automatically and load the new changes.

`npm install nodemon --save-dev `

![image](https://github.com/Mubarokahh/DevOps-Projects/assets/135038657/6f329307-18a2-4f22-a3c9-8dd58e16e6ad)

* In Todo folder open the package.json file. Change the highlighted part of the below screenshot and replace with the code below

 ` "scripts": {
"start": "node index.js",
"start-watch": "nodemon index.js",
"dev": "concurrently \"npm run start-watch\" \"cd client && npm start\""
},`

## Configure Proxy in package.json

Change directory to ‘client’

`cd client`

Open the package.json file

`vi package.json`

Add the key value pair in the package.json file "proxy": "http://localhost:5000"

The whole purpose of adding the proxy configuration in number 3 above is to make it possible to access the application directly from the browser by simply calling the server url like http://localhost:5000 rather than always including the entire path like http://localhost:5000/api/todos

Now, ensure you are inside the Todo directory, and simply run:

`npm run dev`

![image](https://github.com/Mubarokahh/DevOps-Projects/assets/135038657/f0bb58d7-9694-45f7-8644-8b2081379ce5)

Your app should open and start running on localhost:3000

Important note: In order to be able to access the application from the Internet you have to open TCP port 3000 on EC2 by adding a new Security Group rule. 

![image](https://github.com/Mubarokahh/DevOps-Projects/assets/135038657/4b950efc-0b88-458d-a24f-56e5d60405db)

## Creating React Components

One of the advantages of react is that it makes use of components, which are reusable and also makes code modular. For our Todo app, there will be two stateful components and one stateless component. From your Todo directory run

`cd client`

move to the src directory

`cd src`

Inside your src folder create another folder called components

`mkdir components`

Move into the components directory with

`cd components`

![image](https://github.com/Mubarokahh/DevOps-Projects/assets/135038657/d1231df7-85e4-40fb-b113-6cc7480f67db)

Inside ‘components’ directory create three files Input.js, ListTodo.js and Todo.js.

`touch Input.js ListTodo.js Todo.js`


Open Input.js file

`vi Input.js`

Copy and paste the following



`import React, { Component } from 'react';
import axios from 'axios';

class Input extends Component {

state = {
action: ""
}

addTodo = () => {
const task = {action: this.state.action}

    if(task.action && task.action.length > 0){
      axios.post('/api/todos', task)
        .then(res => {
          if(res.data){
            this.props.getTodos();
            this.setState({action: ""})
          }
        })
        .catch(err => console.log(err))
    }else {
      console.log('input field required')
    }

}

handleChange = (e) => {
this.setState({
action: e.target.value
})
}

render() {
let { action } = this.state;
return (
<div>
<input type="text" onChange={this.handleChange} value={action} />
<button onClick={this.addTodo}>add todo</button>
</div>
)
}
}

export default Input
`





  






