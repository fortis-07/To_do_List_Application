# MERN_STACK IMPLEMENTATION ON EC2 INSTANCE

The MERN stack is a powerful framework for building full-stack web applications, consisting of MongoDB, Express.js, React, and Node.js. 

**MongoDB** is a NoSQL database that offers flexible data storage with a schema-less design, making it ideal for dynamic applications. **Express.js** is a minimal web framework for Node.js, facilitating efficient server-side development and robust routing. 

**React**, allows for the creation of dynamic user interfaces through a component-based architecture, enhancing reusability and performance with its virtual DOM. **Node.js** enables server-side execution of JavaScript, providing a non-blocking, event-driven environment that handles multiple connections efficiently. 

Together, these technologies enable developers to write JavaScript across the entire stack, streamlining development processes. The MERN stack is favored for its rapid development capabilities, scalability, and strong community support, making it suitable for a wide range of modern web applications.

## Pre-Setup

**Pre-requisites**
- **AWS Account**: A registered AWS account with permissions to create and manage EC2 instances and associated resources.
- **EC2 and SSH Knowledge**: Familiarity with Amazon EC2, security group configurations, and basic SSH commands for secure server access.
- **SSH Key Pair**: An existing SSH key pair for authenticating and connecting to your EC2 instance, with the private key stored locally for secure access.

 **Provisioning a new EC2 instance and Connection**:
 - Log in to AWS: Go to the AWS Management Console.
- Navigate to EC2: From the services menu, select EC2 (t2-micro, Ubuntu 22.04 LTS).
- Key Pair: Select or create a key pair for SSH access.
Security Group: Open the following ports:
SSH (22)
HTTP (80)

- Launch a new instance:

Once the instance is running, connect to it via SSH.

![WhatsApp Image 2024-09-21 at 17 07 02_1fc5641a](https://github.com/user-attachments/assets/acd830a0-0bb0-478d-ae85-6b62a345e31b)


```bash
ssh -i /path/to/your-key.pem ubuntu@<your-ec2-public-ip>
```

## Step 1: Backend Configuration
- Update ubuntu
```bash
sudo apt update
```
- Upgrade Ubuntu

```bash
sudo apt upgrade
```

- Get the location of Node.js software from ubuntu repositories.

  ```bash
  curl -fsSL https://deb.nodesource.com/setup_18.x | sudo -E bash -
  ```
  ![WhatsApp Image 2024-09-21 at 17 15 21_1fa7ddc6](https://github.com/user-attachments/assets/225e5a06-b463-4482-a847-d092666462a6)
  

- Install Node.js on the server

```bash
sudo apt-get install -y nodejs
```
![WhatsApp Image 2024-09-21 at 17 17 03_8e339fe2](https://github.com/user-attachments/assets/97613c25-efd5-484e-8b22-f8c8ad8687a5)


- Verify the installation

```bash
node -v
npm -v
```

- Create a **Todo** directory

```bash
mkdir Todo
```
- Change our current directory to the newly created one

```bash
cd Todo
```
- Initialise your project
  
![WhatsApp Image 2024-09-21 at 17 24 44_c7e9c94d](https://github.com/user-attachments/assets/e2ccc0fc-7fe9-4ab7-ac1f-19ab27dd6073)

## Step 2: Express.js Installation
- Installation using npm

```bash
npm install express
```

![WhatsApp Image 2024-09-21 at 17 27 28_81f2cdee](https://github.com/user-attachments/assets/c22bc0c5-f9dc-43da-9932-d6f3e7583dd4)

- Create index.js file 

```bash
touch index.js
```
- Run ls to confirm that your index.js file is sucessfully created

- Install dotenv module
  ```bash
  npm install dotenv
  ```
  
 ![WhatsApp Image 2024-09-21 at 17 29 35_8b9f5b7d](https://github.com/user-attachments/assets/ff0e4092-a10a-4665-9ace-a872e4400c7c)
 
  Open the index.js file 'sudo nano index.js' and passte the following:
```bash
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
  console.log(`Server running on port ${port}`);
});
```

Then close with ```Ctrl+0``` followed by ```Ctrl+X```

![WhatsApp Image 2024-09-21 at 17 35 54_ce0132de](https://github.com/user-attachments/assets/23b00e9b-72a4-4c1b-a844-aeef0ee7c31e)


- Next type ```node index.js``` and if everything works it should look like this
  
![WhatsApp Image 2024-09-21 at 17 36 18_483f52bb](https://github.com/user-attachments/assets/a3d22fdb-586b-451f-ab58-27ef36c96dd4)

- We open our browser and access our server
  
![WhatsApp Image 2024-09-21 at 17 36 42_32727728](https://github.com/user-attachments/assets/9a9a0d18-bde1-451e-8767-0298670bb1cd)

## Step 3: Creating Routes
There are three actions that our To-Do application needs to be able to do:

Create a new task
Display list of all tasks
Delete a completed task
Each task will be associated with some particular endpoint and will use different standard HTTP request methods: POST, GET, DELETE. For each task, we need to create routes that will define various endpoints that the To-do app will depend on. So let us create a folder routes Change directory to routes folder, create a file api.js and copy below code in the file.

Run the following commands consecuetively ```mkdir routes``` , ```cd routes``` , ```touch api.js``` , ```sudo nano api.js``` then paste:

```bash
const express = require('express');
const router = express.Router();

router.get('/todos', (req, res, next) => {

});

router.post('/todos', (req, res, next) => {

});

router.delete('/todos', (req, res, next) => {

});

module.exports = router;
```

![WhatsApp Image 2024-09-21 at 17 39 57_e48b20fb](https://github.com/user-attachments/assets/8e656597-a36c-4a33-bfa2-c8727f608759)

## Step 4: Define Models
This app makes use of MongoDB, we need to create a model and a schema.
A Model is at the heart of JavaScript based applicationa, and it's what makes it interactive. It is also use to defined database schema.
The schema is a blueprint of how the database will be constructed.

- To create a schema and a model, install Mongoose which is a Node package that makes working with MongoDB easier.

```bash
npm install mongoose
```

![WhatsApp Image 2024-09-21 at 17 42 34_4793a8f8](https://github.com/user-attachments/assets/7c765ac6-3338-4cd1-9585-1c24c0e90f54)

- Create a new folder in your root directory and name it models. Inside it create a file and name it todo.js with the following code in it:

```bash
$ mkdir models && cd models && touch todo.js
```

![WhatsApp Image 2024-09-21 at 17 43 41_aaeebb53](https://github.com/user-attachments/assets/592c6ce1-ad7e-44bf-a1de-71fe486c98a0)

Open the file created with vim todo.js then paste the code below in the file

```bash
const mongoose = require('mongoose');
const Schema = mongoose.Schema;

const TodoSchema = new Schema({
action: {
type: String,
required: [true, 'The todo text field is required']
}
})

const Todo = mongoose.model('todo', TodoSchema);

module.exports = Todo;
```

![WhatsApp Image 2024-09-21 at 17 44 51_0017553d](https://github.com/user-attachments/assets/f8c26eee-e3db-43a4-8037-cd1dec41c39b)

Next, we need to update our routes file by opening our ```sudo nano api.js``` , delete the code inside with :%d and paste in the code below in ```routes/api.js``` :

```bash
conconst express = require('express');
const router = express.Router();
const Todo = require('../models/todo');

router.get('/todos', (req, res, next) => {


  // This will return all the data, exposing only the id and action field to the client
  Todo.find({}, 'action')
    .the(data => res.json(data))
    .catch(next);
});

router.post('/todos', (req, res, next) => {
  if (req.body.action) {
    Todo.create(req.body)
      .then(data => res.json(data))
      .catch(next);
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

![WhatsApp Image 2024-09-21 at 17 46 36_a20f3b5e](https://github.com/user-attachments/assets/9b246db7-9d8d-46b8-82e3-847ada2887b2)

## Step 5:  MongoDb Database Setup
We require a database to store our data, and we will utilize mLab, which offers MongoDB as a Database as a Service (DBaaS). To facilitate this, please sign up for a free account with shared clusters, suitable for our needs. Sign up here. During the registration process, select AWS as your cloud provider and choose a region that is geographically close to you. Finally, complete the "Get Started" checklist as illustrated in the image below.

![WhatsApp Image 2024-09-19 at 20 04 32_8b2c904c](https://github.com/user-attachments/assets/bf96cc3d-c0dc-405e-bb59-9d5568cc6e9a)

  Click on Browse collection, to create a database.

![WhatsApp Image 2024-09-20 at 12 01 03_a7dbb6ec](https://github.com/user-attachments/assets/9e251d96-d80d-42ba-a8de-a968a12b570f)

 Edit your IP Access List Entry to anywhere to access your database

 ![WhatsApp Image 2024-09-20 at 12 01 35_5a4f7879](https://github.com/user-attachments/assets/60947438-a429-4229-9279-75c9750962d1)
 
Click``` Connect``` to connect your Database with a Driver. Select ```mongoose``` and copy the Connection string.

![WhatsApp Image 2024-09-20 at 12 02 28_0533c5bf](https://github.com/user-attachments/assets/1805aede-6701-4f50-a40a-f4c8de108ac9)

In your Termianl, create a file in your Todo directory and name it ```.env```

```bash
touch .env
sudo nano .env
```
 Paste in our connection string

```bash
DB = 'mongodb+srv://<username>:<password>@<network-address>/<dbname>?retryWrites=true&w=majority'
```

Ensure to update ```<username>``` ```<password>``` ```<network-address>``` and ```<database>``` according to your setup.

Update index.js to reflect the use of .env so that Node.js can connect to the DB.

```bash
sudo nano index.js
```

```bash
       const express = require('express');
       const bodyParser = require('body-parser');
       const mongoose = require('mongoose');
       const routes = require('./routes/api');
       const path = require('path');
       require('dotenv').config();

       const app = express();

       const port = process.env.PORT || 5000;

       //connect to the database
      mongoose.connect(process.env.DB)
     .then(() => console.log('Database connected successfully'))
     .catch(err => console.log('Error connecting to MongoDB:', err));


       //since mongoose promise is depreciated, we overide it with node's promise
       mongoose.Promise = global.Promise;

       app.use((req, res, next) => {
       res.header("Access-Control-Allow-Origin", "\*");
       res.header("Access-Control-Allow-Headers", "Origin, X-Requested-With, Content-Type, Accept");
       next();
       });

       app.use(bodyParser.json());

       app.use('/api', routes);

       app.use((err, req, res, next) =>  {
       console.log(err);
       next();
       });

       app.listen(port, () =>  {
       console.log(`Server running on port ${port}`)
       });
```

![WhatsApp Image 2024-09-21 at 17 53 33_50706cc7](https://github.com/user-attachments/assets/84d85a14-4c59-4995-839a-51ef603af2ed)

Start the server node index.js
You shall see a message "Database connected succesfully":

![WhatsApp Image 2024-09-21 at 18 02 25_326d85b0](https://github.com/user-attachments/assets/7a2f7b95-fae6-491b-8232-632e6ae6d92a)

## Step 5:Testing Backend without Frontend using RESTful API
 
First, we need to download and install postman on our machine: https://www.postman.com/downloads

![WhatsApp Image 2024-09-21 at 18 03 41_91ad8b53](https://github.com/user-attachments/assets/56fdaa90-77da-4bf0-bbb9-42848753920a)

So far we have written backend part of our To-Do application, and configured a database, but we do not have a frontend UI yet. We need ReactJS code to achieve that. But during development, we will need a way to test our code using RESTfulL API. Therefore, we will need to make use of some API development client to test our code. We should test all the API endpoints and make sure they are working. For the endpoints that require body, you should send JSON back with the necessary fields since it’s what we setup in our code. Now open your Postman, create a POST request to the API http://:5000/api/todos. This request sends a new task to our To-Do list so the application could store it in the database.

Note: make sure your set header key Content-Type as application/json

![WhatsApp Image 2024-09-21 at 18 09 23_a0ba9840](https://github.com/user-attachments/assets/d36a9b48-faed-4026-b564-f08b22d826ad)

![image](https://github.com/user-attachments/assets/db6f4e40-83e9-4948-812d-4b48baa4f48a)

## Step 6: Front-end Set-up

In the Todo directory, run

```bash
npx create-react-app client
```

Install **concurrently** Concurrently is used to run more than one command simultaneously from the same terminal window.

```bash
npm install concurrently --save-dev
```

- Install nodemon Nodemon is used to run the server and monitor it as well. If there is any change in the server code, Nodemon will restart it automatically with the new changes.

```bash
npm install nodemon --save-dev
```

![WhatsApp Image 2024-09-21 at 20 04 13_cd03f451](https://github.com/user-attachments/assets/e8fda6b6-a411-4295-8d52-86fd7efd7563)

- Open package.json file

```bash
sudo nano package.json
```
- Paste this in the Scripts block:

{
  "name": "todo",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
  "start": "node index.js",
  "start-watch": "nodemon index.js",
  "dev": "concurrently \"npm run start-watch\" \"cd client && npm start\""
 },
  "author": "",
  "license": "ISC",
  "dependencies": {
    "dotenv": "^16.4.5",
    "express": "^4.19.2",
    "mongoose": "^8.3.4"
  },
  "devDependencies": {
    "concurrently": "^8.2.2",
    "nodemon": "^3.1.0"
  }
}

![WhatsApp Image 2024-09-21 at 20 11 33_eb7cbe73](https://github.com/user-attachments/assets/c416f648-cca5-4a43-bb84-80bc236f1026)

- Configure Proxy in package.json located in the Client directory.

```bash
cd client
```

```bash
sudo nano package.json
```
then add

```bash
{
  
  "proxy": "http://localhost:5000"
}

```

- Run  ```npm run dev```  in the todo directory.

![WhatsApp Image 2024-09-23 at 15 14 21_1473b694](https://github.com/user-attachments/assets/87c99dc9-da83-4820-9ab5-44cc61240cd9)

## Step 7 — React Components Set-up
One of the advantages of react is that it makes use of components, which are reusable and also makes code modular. For our Todo app, there will be two stateful components and one stateless component. From your Todo directory run
cd client

Move to the “src” directory

```bash
cd src```
```

- In your src folder, create another folder called “components”

```bash
mkdir components
```

- Move into the components directory

```bash
cd components
```
- In the ‘components’ directory create three files ```Input.js```, ```ListTodo.js``` and ```Todo.js```.

```bash
touch Input.js ListTodo.js Todo.js
```

![WhatsApp Image 2024-09-23 at 15 20 18_11dbd8e6](https://github.com/user-attachments/assets/e32999dd-c77e-4d0e-82d9-334967b8a184)

- Open Input.js file

```bash
sudo nano Input.js
```
- Paste this in the Script block:

```bash
import React, { Component } from 'react';
import axios from 'axios';

class Input extends Component {
  state = {
    action: " "
  }

  addTodo = () => {
    const task = { action: this.state.action }

    if (task.action && task.action.length > 0) {
      axios
        .post('/api/todos', task)
        .then((res) => {
          if (res.data) {
            this.props.getTodos();
            this.setState({ action: "" })
          }
        })
        .catch((err) => console.log(err));
    } else {
      console.log('input field required');
    }
  }

  handleChange = (e) => {
    this.setState({
      action: e.target.value,
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
- To make use of axios, which is a Promise-based HTTP client for the browser and Node.js, you will need to navigate to your client directory from your terminal:

```bash
cd client
```
```bash
npm install axios
```
![WhatsApp Image 2024-09-23 at 15 22 31_9dd559c6](https://github.com/user-attachments/assets/47247071-3d16-4bff-a45b-82ce3d25d12f)

- Go back to the component directory

```bash
 cd src/components
````
After that open the ```ListTodo.js``` 

```bash
sudo nano ListTodo.js
```
Copy and paste the following code:

```bash
import React, { Component } from 'react';
import axios from 'axios';

import Input from './Input';
import ListTodo from './ListTodo';

class Todo extends Component {
  state = {
    todos: []
  }

  componentDidMount() {
    this.getTodos();
  }

  getTodos = () => {
    axios.get('/api/todos')
      .then(res => {
        if (res.data) {
          this.setState({
            todos: res.data
          });
        }
      })
      .catch(err => console.log(err));
  }

  deleteTodo = (id) => {
    axios.delete(`/api/todos/${id}`)
      .then(res => {
        if (res.data) {
          this.getTodos();
        }
      })
      .catch(err => console.log(err));
  }

  render() {
    let { todos } = this.state;
    return (
      <div>
        <h1>My Todo(s)</h1>
        <Input getTodos={this.getTodos} />
        <ListTodo todos={todos} deleteTodo={this.deleteTodo} />
      </div>
    );
  }
}
export default Todo;
```

![WhatsApp Image 2024-09-23 at 15 26 23_3bdccfef](https://github.com/user-attachments/assets/25fe3fd3-338d-449e-8df6-a352ee5ac19e)


- Adjust the React code by Deleting the logo and adjust App.js to look like this:

```bash
cd ..
```

- Open App.js with nano text editor

```bash
sudo nano App.js
```
Paste this in the Script block:

```bash
import React from 'react';
import Todo from './components/Todo';
import './App.css';

const App = () => {
  return (
    <div className="App">
      <Todo />
    </div>
  );
};

export default App;
```

- In the src directory, open the App.css

```bash
sudo nano App.css
```
- Paste the following code intothe Script block:

```bash
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
    width: 100%;
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
 In the src directory, open the index.css

vim index.css
Paste this:

body {
  margin: 0;
  padding: 0;
  font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", "Roboto", "Oxygen", "Ubuntu", "Cantarell", "Fira Sans", "Droid Sans", "Helvetica Neue", sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  box-sizing: border-box;
  background-color: #282c34;
  color: #787a80;
}

code {
  font-family: source-code-pro, Menlo, Monaco, Consolas, "Courier New", monospace;
}
- Go back to the Todo directory

```bash
cd  ../..
```

- On the Todo directory run

```bash
npm run dev
```

- React build would compile sucessfully and both backend and front end will run concurrently through the ports we earlier opened

  ![WhatsApp Image 2024-09-23 at 19 24 28_8ddcb1fc](https://github.com/user-attachments/assets/7564353c-27f6-404a-bf25-d4bcf36fef77)


![image](https://github.com/user-attachments/assets/d986f05e-a236-4281-bb26-298311c7a0e0)
