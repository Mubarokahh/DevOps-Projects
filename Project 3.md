# MERN Stack Impementation
This project aims to implement web solution based on MERN technology stack in the AWS stack. The MERN acronym stand for MongoDB, ExpressJS, ReactJS, NodeJS.

## Step 1: Setting up a virtual environment in AWS Cloud
I set up an EC2 instance in AWS.I then open my mac terminal and ran the SSH comands in order to connect to the server I have running in AWS cloud using my private key.Upon connecting to the virtual environment, i configured the virtual server (Ubuntu) that is now running on my mac terminal using the following commands:
1. `sudo apt update`
2. `suso apt upgrade`

![image](https://github.com/Mubarokahh/DevOps-Projects/assets/135038657/6a80e081-f961-4f33-b84b-d8bb501b0b24)

  To unveil the location of NodeJS SOFTWARE FROM Ubuntu repositories
 ` curl -fsSL https://deb.nodesource.com/setup_18.x | sudo -E bash -`

  ## Step 2: Installing NodeJS on the Server
  
  * To install NodeJS, run: 
  This command installs both NodeJS and NPM (Node Package Manager)

  ![image](https://github.com/Mubarokahh/DevOps-Projects/assets/135038657/d5e54d63-89a9-41b2-a2da-bc9811a3aba6)


  ## STEP 3:Application Code Setup
  * Creating a new directory: ` mkdir Todo`
  * To verify that the directory is created: `ls`
  * Changing the current directory to work in Todo directory:  `cd Todo`
  * Initializing my project which will create package.json with this command:  `npm init`
  * Following the prompt and accept to write out the package.json file

    ![image](https://github.com/Mubarokahh/DevOps-Projects/assets/135038657/49c9efa3-e275-46d9-9b14-b734c0e1102f)


  ## STEP 4:Installing ExpressJS
  * To use express, install it via npm :` npm install express`
  * Creating a new file index.js:` touch index.js`
  * Installing the dotenv module:` npm install dotenv`

   ![image](https://github.com/Mubarokahh/DevOps-Projects/assets/135038657/ff6d7775-48ab-4fcd-be92-c51063d36d6a)


  * To edit the index.js file creted: vim index.js
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

const mongoose = require('mongoose');
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

module.exports = Todo;

* Updating the routes file in the routes directory in order to work with the new model created and entering the code:

    const express = require ('express');
    const router = express.Router();
    const Todo = require('../models/todo');

    router.get('/todos', (req, res, next) => {

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

    module.exports = router;

    ## STEP 7: Launching MongoDG Database using mLab
    The mLab provides MongoDB database as a service solution (DBaaS). To do this
    * Creating a mLab account 
    * Setting up  a MongoDB database and collection inside mLab
    * Creating a .env file in Todo directory: touch .env
    * Opening the file: $ vi .env
    * Adding the connection string to access the database embeded in it
     DB = 'mongodb+srv://<username>:<password>@<network-address>/<dbname>?retryWrites=true&w=majority' 
     SIDENOTE: replace the username, password and network-address, database name with the credentials used in creating the mongodb database

     mongo image
     Updating the index.js file to reflect the use of .env so that Node.js can connect to the database
     * Open the file with: vim index.js
     * Delete the current script with: :%d
     * Paste the following code:
     const express = require('express');
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
});

 * Start the derver with: node index.js
 * 



