# Simple To-Do application on MERN Web Stack

### MERN STACK
MERN stack is one of the widely used web development technology stacks in use today. The technology stack is a set of frameworks and tools used to develop a software product. They are very specifically chosen to work together in creating a well-functioning software.

- MERN (MongoDB, ExpressJS, ReactJS, NodeJS)
- LAMP (Linux, Apache, MySQL, PHP) 
- MEAN (MongoDB, ExpressJS, AngularJS, NodeJS)

### What is MERN stack?
MERN stack is a web development framework. It is made up of four key technologies as its working components: 
- MongoDB - document database. It is a document-based, open source and a no-SQL database used to store the application data 
- Express(.js) - Node.js web framework. A web application and server side framework for Node.js. A framework on the other side is a compilation of code libraries that a developer can use to speed up development work, rather than writing code completely from scratch.
- React(.js) - a client-side JavaScript framework. A javascript front-end library for building user interfaces.
- Node(.js) - the premier JavaScript web server. Javascript run-time environment that executes javascript code outside of a browser (such as a server, machine).

In MERN stack, every line of code is written in JavaScript. This is a programming language that’s used everywhere, both for client-side code and server-side code. With one language across tiers, there’s no need for context switching.

For tech stack with multiple programming languages, developers have to figure out how to interface them together. With the JavaScript stack, developers only need to be proficient in JavaScript and JSON. Overall, using the MERN stack enables developers to build highly efficient web applications.

MERN is a full-stack architecture, following the traditional 3-tier architectural pattern, including the front-end display tier (React.js), application tier (Express.js and Node.js), and database tier (MongoDB). It allows an easy construction of  a 3-tier architecture (frontend, backend, database) entirely using JavaScript and JSON.

![](./images/mern.png)

As shown on the illustration above, a user interacts with the ReactJS UI components at the application front-end residing in the browser. This frontend is served by the application backend residing in a server, through ExpressJS running on top of NodeJS.

Any interaction that causes a data change request is sent to the NodeJS based Express server, which grabs data from the MongoDB database if required, and returns the data to the frontend of the application, which is then presented to the user.

In order to complete this project you will need an AWS account and a virtual server with Ubuntu Server OS.

If you do not have an AWS account - go back to [WEB STACK IMPLEMENTATION](https://github.com/samuelbartels20/web-stack-implementation) Step 0 to sign in to AWS free tier account and create a new EC2 Instance of t2.nano family with Ubuntu Server 20.04 LTS (HVM) image. Remember, you can have multiple EC2 instances, but make sure you **STOP** the ones you are not working with at the moment to save available free hours.

**Hint #1**: When you create your EC2 Instances, you can add Tag “Name” to it with a value that corresponds to a current project you are working on - it will be reflected in the name of the EC2 Instance. Like this:

![](./images/EC2_tag.png)

[MobaXterm](https://mobaxterm.mobatek.net/download.html) is an advanced multirotocol and multitool terminal for Windows.

Download and launch MobaxTerm, create a new SSH session with , ‘ubuntu’ as username and your private key (.pem file) like this:

![](./images/MobaXterm.gif)

To deploy a simple To-Do application that creates To-Do lists like this:

![](./images/mern.gif)

##  Backend configuration
Update ubuntu

```
sudo apt update
```
![](./images/ec2.png)

![](./images/update.png)

Upgrade ubuntu
```
sudo apt upgrade
```
![](./images/upgrade.png)

Lets get the location of Node.js software from [Ubuntu repositories](https://github.com/nodesource/distributions#deb).

```
curl -sL https://deb.nodesource.com/setup_12.x | sudo -E bash -
```

![](./images/side.png)

### Install Node.js on the server
Install Node.js with the command below
```
sudo apt-get install -y nodejs
```
![](./images/node.png)

**Note**: The command above installs both <span style="color:red">nodejs</span>. and <span style="color:red">npm</span>. [NPM](https://www.npmjs.com/) is a package manager for Node like <span style="color:red">apt</span> for Ubuntu, it is used to install Node modules & packages and to manage dependency conflicts.

Verify the node installation with the command below
```
node -v 
```
![](./images/version.png)

Verify the node installation with the command below
```
npm -v 
```
![](./images/n-version.png)

### Application Code Setup
Create a new directory for your To-Do project:
```
$ mkdir Todo
```

Run the command below to verify that the <span style="color:red">Todo</span> directory is created with <span style="color:red">ls</span> command

**TIP**: In order to see some more useful information about files and directories, you can use following combination of keys <span style="color:red">ls -lih</span> - it will show you different properties and size in human readable format. You can learn more about different useful keys for <span style="color:red">ls </span> command with <span style="color:red">ls --help</span>.

Now change your current directory to the newly created one:
```
$ cd Todo
```

Next, you will use the command <span style="color:red">npm init</span> to initialise your project, so that a new file named <span style="color:red">package.json</span> will be created. This file will normally contain information about your application and the dependencies that it needs to run. Follow the prompts after running the command. You can press <span style="color:red">Enter</span> several times to accept default values, then accept to write out the <span style="color:red">package.json</span> file by typing <span style="color:red">yes</span>.

![](./images/npm.png)

Run the command <span style="color:red">ls</span> to confirm that you have package.json file created.

![](./images/ls.png)

### Install ExpressJS
Remember that Express is a framework for Node.js, therefore a lot of things developers would have programmed is already taken care of out of the box. Therefore it simplifies development, and abstracts a lot of low level details. For example, Express helps to define routes of your application based on HTTP methods and URLs.

To use express, install it using npm:
```
$ npm install express
```
![](./images/crop.png)

Now create a file <span style="color:red">index.js</span> with the command below
```
$ touch index.js
```
Run <span style="color:red">ls</span> to confirm that your index.js file is successfully created

Install the <span style="color:red">dotenv</span> module
```
npm install dotenv
```

Open the index.js file with the command below
```
$ vim index.js
```

Type the code below into it and save. Do not get overwhelmed by the code you see. For now, simply paste the code into the file.
```
const express = require('express');
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
console.log(`Server running on port ${port}`)
});
```

![](./images/code.png)

Notice that we have specified to use port <span style="color:red">5000</span> in the code. This will be required later when we go on the browser.

Use <span style="color:red">:w</span> to save in vim and use <span style="color:red">:qa</span> to exit vim

Now it is time to start our server to see if it works. Open your terminal in the same directory as your index.js file and type:
```
$ node index.js
```

If every thing goes well, you should see **Server running on port 5000** in your terminal.

![](./images/cli.png)

Now we need to open this port in EC2 Security Groups. Refer to [WEB STACK IMPLEMENTATION](https://github.com/samuelbartels20/web-stack-implementation) Step 1 – Installing the Nginx Web Server. There we created an inbound rule to open TCP port 80, you need to do the same for port 5000, like this:

![](./images/codr.png)

Open up your browser and try to access your server’s Public IP or Public DNS name followed by port 5000:
```
http://<PublicIP-or-PublicDNS>:5000
```

Quick reminder how to get your server’s Public IP and public DNS name:
- You can find it in your AWS web console in EC2 details
- Run <span style="color:red">curl -s http://169.254.169.254/latest/meta-data/public-ipv4</span> for Public IP address or <span style="color:red">curl -s http://169.254.169.254/latest/meta-data/public-hostname</span> for Public DNS name.

![](./images/exp.png)

### Routes
There are three actions that our To-Do application needs to be able to do:
- Create a new task
- Display list of all tasks
- Delete a completed task

Each task will be associated with some particular endpoint and will use different standard [HTTP request methods](https://developer.mozilla.org/en-US/docs/Web/HTTP/Methods): POST, GET, DELETE.

For each task, we need to create <span style="color:red">routes</span> that will define various endpoints that the <span style="color:red">To-do</span> app will depend on. So let us create a folder <span style="color:red">routes</span>
```
$ mkdir routes
```

**Tip**: You can open multiple shells in Putty or Linux/Mac to connect to the same EC2

Change directory to <span style="color:red">routes</span> folder.
```
$ cd routes
```

Now, create a file <span style="color:red">api.js</span> with the command below
```
touch api.js
```

Open the file with the command below
```
vim api.js
```

Copy below code in the file. (Do not be overwhelmed with the code)
```
const express = require ('express');
const router = express.Router();

router.get('/todos', (req, res, next) => {

});

router.post('/todos', (req, res, next) => {

});

router.delete('/todos/:id', (req, res, next) => {

})

module.exports = router;
```

### Models
Now comes the interesting part, since the app is going to make use of [Mongodb](https://www.mongodb.com/) which is a NoSQL database, we need to create a model.

A model is at the heart of JavaScript based applications, and it is what makes it interactive.

We will also use models to define the database schema . This is important so that we will be able to define the fields stored in each Mongodb document. ***(Seems like a lot of information, but not to worry, everything will become clear to you over time. I promise!!!)***

In essence, the Schema is a blueprint of how the database will be constructed, including other data fields that may not be required to be stored in the database. These are known as virtual properties

To create a Schema and a model, install [mongoose](https://mongoosejs.com/) which is a Node.js package that makes working with mongodb easier.

Change directory back Todo folder with <span style="color:red">cd ..</span> and install Mongoose
```
$ npm install mongoose
```

Create a new folder with <span style="color:red">mkdir models</span> command

Change directory into the newly created ‘models’ folder with <span style="color:red">cd models</span> .

Inside the models folder, create a file and name it <span style="color:red">todo.js</span> 
```
touch todo.js
```

**Tip**: All three commands above can be defined in one line to be executed consequently with help of <span style="color:red">&&</span>  operator, like this:
```
mkdir models && cd models && touch todo.js
```

Open the file created with vim todo.js then paste the code below in the file:
```
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
```
![](./images/imp.png)

Now we need to update our routes from the file <span style="color:red">api.js</span> in ‘routes’ directory to make use of the new model.

In Routes directory, open api.js with <span style="color:red">vim api.js</span>, delete the code inside with :%d command and paste there code below into it then save and exit
```
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
```

![](./images/cream.png)

### MongoDB Database
We need a database where we will store our data. For this we will make use of mLab. mLab provides MongoDB database as a service solution ([DBaaS](https://en.wikipedia.org/wiki/Cloud_database)), so to make life easy, you will need to sign up for a shared clusters free account, which is ideal for our use case. [Sign up here](https://www.mongodb.com/atlas-signup-from-mlab). Follow the sign up process, select **AWS** as the cloud provider, and choose a region near you.

Complete a get started checklist as shown on the image below

![](./images/MLab-dashboard.png)

![](./images/mlab.png)

![](./images/mlab2.png)

![](./images/mlab3.png)

![](./images/mlab4.png)

Allow access to the MongoDB database from anywhere (Not secure, but it is ideal for testing)

**IMPORTANT NOTE** In the image below, make sure you change the time of deleting the entry from 6 Hours to 1 Week

![](./images/mlab6.png)

![](./images/mlab5.png)

![](./images/mlab7.png)

![](./images/mlab8.png)

Create a MongoDB database and collection inside mLab

![](./images/mongo.png)

![](./images/mongo2.png)

![](./images/mongo3.png)

![](./images/mongo4.png)

![](./images/mongo6.png)

In the <span style="color:red">index.js</span> file, we specified <span style="color:red">process.env</span> to access environment variables, but we have not yet created this file. So we need to do that now.

Create a file in your <span style="color:red">Todo</span> directory and name it <span style="color:red">.env</span>.
```
touch .env
vi .env
```

Add the connection string to access the database in it, just as below:
```
DB = 'mongodb+srv://<username>:<password>@<network-address>/<dbname>?retryWrites=true&w=majority'
```

![](./images/mongopass.png)

Ensure to update <span style="color:red">username</span>, <span style="color:red">password</span>, <span style="color:red">network-address</span> and <span style="color:red">database</span> according to your setup

Here is how to get your connection string
![](./images/string.png)

![](./images/mongo7.png)

![](./images/mongo8.png)

![](./images/mongo9.png)

![](./images/mongo10.png)

![](./images/mongo11.png)

Now we need to update the <span style="color:red">index.js</span> to reflect the use of <span style="color:red">.env</span> so that Node.js can connect to the database.

Simply delete existing content in the file, and update it with the entire code below.

To do that using <span style="color:red">vim</span>, follow below steps
- Open the file with <span style="color:red">vim index.js</span>
- Press <span style="color:red">esc</span>
- Type <span style="color:red">:</span>
- Type <span style="color:red">%d</span>
- Hit <span style="color:red">Enter</span>

The entire content will be deleted, then,

- Press <span style="color:red">i</span> to enter the insert mode in vim
- Now, paste the entire code below in the file.
```
const express = require('express');
const bodyParser = require('body-parser');
const mongoose = require('mongoose');
const routes = require('./routes/api');
const path = require('path');
require('dotenv').config();

const app = express();

const port = process.env.PORT || 5000;

//connect to the database
mongoose.connect(process.env.DB, { useNewUrlParser: true })
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
```


Using environment variables to store information is considered more secure and best practice to separate configuration and secret data from the application, instead of writing connection strings directly inside the  <span style="color:red">index.js</span> application file.

Start your server using the command:
```
node index.js
```

![](./images/imagine.png)

You shall see a message **‘Database connected successfully’**, if so - we have our backend configured. Now we are going to test it.

### Testing Backend Code without Frontend using RESTful API
So far we have written backend part of our <span style="color:red">To-Do</span> application, and configured a database, but we do not have a frontend UI yet. We need ReactJS code to achieve that. But during development, we will need a way to test our code using RESTfulL API. Therefore, we will need to make use of some API development client to test our code.

In this project, we will use [Postman](https://www.getpostman.com/) to test our API. Click Install Postman to download and [install postman](https://www.getpostman.com/downloads/) on your machine.

Click [HERE](https://www.youtube.com/watch?v=FjgYtQK_zLE) to learn how perform [CRUD operations](https://en.wikipedia.org/wiki/Create,_read,_update_and_delete) on Postman

You should test all the API endpoints and make sure they are working. For the endpoints that require body, you should send JSON back with the necessary fields since it’s what we setup in our code.

Now open your Postman, create a POST request to the API <span style="color:red">http://<PublicIP-or-PublicDNS>:5000/api/todos</span>. This request sends a new task to our To-Do list so the application could store it in the database.

![](./images/crude.png)

**Note**: make sure your set header key <span style="color:red">Content-Type</span> as <span style="color:red">application/json</span>

![](./images/postman_header.png)

Create a GET request to your API on <span style="color:red">http://<PublicIP-or-PublicDNS>:5000/api/todos</span>. This request retrieves all existing records from out To-do application (backend requests these records from the database and sends it us back as a response to GET request).

![](./images/get.png)

**Optional task**: Try to figure out how to send a DELETE request to delete a task from out To-Do list.

**Hint**: To delete a task - you need to send its ID as a part of DELETE request.

By now you have tested backend part of our To-Do application and have made sure that it supports all three operations we wanted:
- [x] Display a list of tasks - HTTP GET request
- [x] Add a new task to the list - HTTP POST request
- [x] Delete an existing task from the list - HTTP DELETE request

###  Frontend creation
Since we are done with the functionality we want from our backend and API, it is time to create a user interface for a Web client (browser) to interact with the application via API. To start out with the frontend of the To-do app, we will use the <span style="color:red">create-react-app</span> command to scaffold our app.

In the same root directory as your backend code, which is the Todo directory, run:
```
$ npx create-react-app client
```

![](./images/react.png)

This will create a new folder in your <span style="color:red">Todo</span> directory called <span style="color:red">client</span>, where you will add all the react code.

### Running a React App
Before testing the react app, there are some dependencies that need to be installed.

- Install [concurrently](https://www.npmjs.com/package/concurrently). It is used to run more than one command simultaneously from the same terminal window.
```
$ npm install nodemon --save-dev
```

![](./images/con.png)

- Install [nodemon](https://www.npmjs.com/package/nodemon). It is used to run and monitor the server. If there is any change in the server code, nodemon will restart it automatically and load the new changes.
```
$ npm install nodemon --save-dev
```

![](./images/nodemon.png)

- In <span style="color:red">Todo</span> folder open the <span style="color:red">package.json</span> file. Change the highlighted part of the below screenshot and replace with the code below.
```
"scripts": {
"start": "node index.js",
"start-watch": "nodemon index.js",
"dev": "concurrently \"npm run start-watch\" \"cd client && npm start\""
},
```

![](./images/test.png)

### Configure Proxy in <span style="color:red">package.json</span>
- Change directory to ‘client’
```
cd client
```
- Open the <span style="color:red">package.json</span> file
```
vi package.json
```

![](./images/package.png)
- Add the key value pair in the package.json file span style="color:red">"proxy": "http://localhost:5000"</span>.

![](./images/proxy.png)

The whole purpose of adding the proxy configuration in number 3 above is to make it possible to access the application directly from the browser by simply calling the server url like<span style="color:red"> http://localhost:5000</span> rather than always including the entire path like <span style="color:red">http://localhost:5000/api/todos</span>

Now, ensure you are inside the <span style="color:red">Todo</span> directory, and simply do:
```
npm run dev
```
![](./images/dev.png)

Your app should open and start running on <span style="color:red">Public-IP:3000</span>
![](./images/re.png)


**Important note**: In order to be able to access the application from the Internet you have to open TCP port 3000 on EC2 by adding a new Security Group rule. You already know how to do it.

### Creating your React Components
One of the advantages of react is that it makes use of components, which are reusable and also makes code modular. For our Todo app, there will be two stateful components and one stateless component. From your Todo directory run
```
cd client
```

move to the src directory
```
cd src
```
Inside your <span style="color:red">src</span> folder create another folder called <span style="color:red">components</span>
```
mkdir components
```

Move into the components directory with
```
cd components
```

Inside ‘components’ directory create three files <span style="color:red">Input.js</span>, <span style="color:red">ListTodo.js</span> and <span style="color:red">Todo.js</span>
```
touch Input.js ListTodo.js Todo.js
```

Open <span style="color:red">Input.js</span> file
```
vi Input.js
```

Copy and paste the following
```
import React, { Component } from 'react';
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
```

![](./images/finish.png)

To make use of [Axios](https://github.com/axios/axios), which is a Promise based HTTP client for the browser and node.js, you need to cd into your client from your terminal and run yarn add axios or npm install axios.

Move to the src folder
```
cd ..
```

Install Axios
```
$ npm install axios
```

Go to ‘components’ directory
```
cd src/components
```

After that open your <span style="color:red">ListTodo.js</span>
```
vi ListTodo.js
```

In the <span style="color:red">ListTodo.js</span> copy and paste the following code
```
import React from 'react';

const ListTodo = ({ todos, deleteTodo }) => {

return (
<ul>
{
todos &&
todos.length > 0 ?
(
todos.map(todo => {
return (
<li key={todo._id} onClick={() => deleteTodo(todo._id)}>{todo.action}</li>
)
})
)
:
(
<li>No todo(s) left</li>
)
}
</ul>
)
}

export default ListTodo
```

Then in your <span style="color:red">Todo.js</span> file you write the following code
```
import React, {Component} from 'react';
import axios from 'axios';

import Input from './Input';
import ListTodo from './ListTodo';

class Todo extends Component {

state = {
todos: []
}

componentDidMount(){
this.getTodos();
}

getTodos = () => {
axios.get('/api/todos')
.then(res => {
if(res.data){
this.setState({
todos: res.data
})
}
})
.catch(err => console.log(err))
}

deleteTodo = (id) => {

    axios.delete(`/api/todos/${id}`)
      .then(res => {
        if(res.data){
          this.getTodos()
        }
      })
      .catch(err => console.log(err))

}

render() {
let { todos } = this.state;

    return(
      <div>
        <h1>My Todo(s)</h1>
        <Input getTodos={this.getTodos}/>
        <ListTodo todos={todos} deleteTodo={this.deleteTodo}/>
      </div>
    )

}
}

export default Todo;
```

We need to make little adjustment to our react code. Delete the logo and adjust our <span style="color:red">App.js</span> to look like this.

Move to the src folder
```
cd ..
```

Make sure that you are in the src folder and run
```
vi App.js
```

Copy and paste the code below into it
```
import React from 'react';

import Todo from './components/Todo';
import './App.css';

const App = () => {
return (
<div className="App">
<Todo />
</div>
);
}

export default App;
```

After pasting, exit the editor.

In the src directory open the <span style="color:red">App.css</span>
```
vi App.css
```

Then paste the following code into <span style="color:red">App.css</span>:
```
.App {
text-align: center;
font-size: calc(10px + 2vmin);
width: 60%;
margin-left: auto;
margin-right: auto;
}

input {
height: 40px;
width: 50%;
border: none;
border-bottom: 2px #101113 solid;
background: none;
font-size: 1.5rem;
color: #787a80;
}

input:focus {
outline: none;
}

button {
width: 25%;
height: 45px;
border: none;
margin-left: 10px;
font-size: 25px;
background: #101113;
border-radius: 5px;
color: #787a80;
cursor: pointer;
}

button:focus {
outline: none;
}

ul {
list-style: none;
text-align: left;
padding: 15px;
background: #171a1f;
border-radius: 5px;
}

li {
padding: 15px;
font-size: 1.5rem;
margin-bottom: 15px;
background: #282c34;
border-radius: 5px;
overflow-wrap: break-word;
cursor: pointer;
}

@media only screen and (min-width: 300px) {
.App {
width: 80%;
}

input {
width: 100%
}

button {
width: 100%;
margin-top: 15px;
margin-left: 0;
}
}

@media only screen and (min-width: 640px) {
.App {
width: 60%;
}

input {
width: 50%;
}

button {
width: 30%;
margin-left: 10px;
margin-top: 0;
}
}
```

In the src directory open the <span style="color:red">index.css</span>
```
vim index.css
```

Copy and paste the code below:
```
body {
margin: 0;
padding: 0;
font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", "Roboto", "Oxygen",
"Ubuntu", "Cantarell", "Fira Sans", "Droid Sans", "Helvetica Neue",
sans-serif;
-webkit-font-smoothing: antialiased;
-moz-osx-font-smoothing: grayscale;
box-sizing: border-box;
background-color: #282c34;
color: #787a80;
}

code {
font-family: source-code-pro, Menlo, Monaco, Consolas, "Courier New",
monospace;
}
```

Go to the Todo directory
```
cd ../..
```

When you are in the Todo directory run:
```
npm run dev
```

Assuming no errors when saving all these files, our To-Do app should be ready and fully functional with the functionality discussed earlier: creating a task, deleting a task and viewing all your tasks.

![](./images/png.png)

![](./images/todo.png)

In this Project, you have created a simple To-Do and deployed it to MERN stack. You wrote a frontend application using React.js that communicates with a backend application written using Expressjs. You also created a Mongodb backend for storing tasks in a database.

Credits:
- [This guide was inspired by Digital Ocean](https://www.digitalocean.com/community/tutorials/getting-started-with-the-mern-stack)