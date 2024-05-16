USE CASE
Challenge: DTC is a rapidly growing online clothing store experiencing issues with their current Content Delivery Network (CDN). As their product catalog and customer base expand, the existing CDN struggles to deliver high-resolution images and videos efficiently. This results in slow loading times, impacting customer experience and potentially leading to lost sales.
Solution: Migrate to a more robust and scalable CDN platform on a cloud provider like Google Cloud Platform (GCP).

1. Setting Up the Development Environment:
Prerequisites:
•	An AWS account with appropriate permissions.
•	Node.js and npm installed on your local development machine.
Ubuntu Server Setup:
1.	Create an EC2 Instance:
o	Login to the AWS Management Console and navigate to the EC2 service.
o	Choose "Launch Instance."
o	Select an appropriate Amazon Machine Image (AMI). For this example, we'll use "Ubuntu Server 20.04 LTS."
o	Choose an instance type. For development, a free tier t2.micro instance is sufficient.
o	Configure security groups to allow inbound SSH access on port 22.
o	Launch the instance and download the key pair (.pem file).
2.	Connect to the Instance:
o	Use an SSH client like PuTTY to connect to your instance using the public DNS address and your downloaded key pair.
2. Backend Development:
Project Setup:
1.	Create a Project Directory:
Bash
mkdir todo-api
cd todo-api
Use code with caution.
content_copy
2.	Initialize Project:
Bash
npm init -y
Use code with caution.
content_copy
3.	Install Dependencies:
Bash
npm install express mongoose dotenv cors body-parser
Use code with caution.
content_copy
o	express: Web framework for building the backend API.
o	mongoose: ODM (Object Data Modeling) library for interacting with MongoDB.
o	dotenv: Loads environment variables from a .env file for sensitive information.
o	cors: Enables Cross-Origin Resource Sharing (CORS) for frontend requests.
o	body-parser: Parses incoming request bodies.
4.	Create .env File: (Store sensitive information outside of version control)
Bash
touch .env
Use code with caution.
content_copy
5.	Configure Database Connection:
o	Create a MongoDB database instance on AWS (e.g., DocumentDB).
o	Obtain the connection string from your MongoDB instance and add it to the .env file.
6.	MONGO_URI=mongodb://<username>:<password>@<host>:<port>/<database>
Backend Code (index.js):
1.	Import necessary modules:
JavaScript
const express = require('express');
const mongoose = require('mongoose');
const dotenv = require('dotenv');
const cors = require('cors');
const bodyParser = require('body-parser');
Use code with caution.
content_copy
2.	Load environment variables:
JavaScript
dotenv.config();
Use code with caution.
content_copy
3.	Connect to MongoDB:
JavaScript
mongoose.connect(process.env.MONGO_URI, {
  useNewUrlParser: true,
  useUnifiedTopology: true
})
.then(() => console.log('MongoDB Connected'))
.catch(err => console.error(err));
Use code with caution.
content_copy
4.	Define the Mongoose schema for tasks:
JavaScript
const TaskSchema = new mongoose.Schema({
  title: {
    type: String,
    required: true
  },
  completed: {
    type: Boolean,
    default: false
  }
});

const Task = mongoose.model('Task', TaskSchema);
Use code with caution.
content_copy
5.	Create API endpoints:
o	GET /todos: Get all tasks.
o	POST /todos: Create a new task.
o	PUT /todos/:id: Update a task by ID.
o	DELETE /todos/:id: Delete a task by ID.
JavaScript
app.use(cors());
app.use(bodyParser.json());

app.get('/todos', async (req, res) => {
  try {
    const tasks = await Task.find();
    res.json(tasks);
  } catch (err) {
    res.status(500).json({ message: err.message });
  }
});

// ... Implement similar logic for other API endpoints ...
Use code with caution.
content_copy
6.	Start the server:
JavaScript
const port = process.env.PORT || 5000;

app.listen(port, () => console.log(`Server started on port ${port}`));
Use code with caution.
content_copy
3. Testing the Backend API:
1.	Use a tool like Postman to send test requests to the API endpoints.
2.	Verify that you can successfully create, read, update, and delete tasks.
**4. Frontend Development
Sources 
info
1.	github.com/Lemoncode/master-frontend-lemoncode 
2.	stackoverflow.com/questions/60093457/express-router-unable-to-route-to-another-folder 
3.	forum.freecodecamp.org/t/cant-pass-challenge-create-and-save-record-of-a-model-mongoose-and-mongodb/497759 
4.	github.com/RashidCHAB/todo_fullstack 
share
more_vert
 
edit
CONTINUE
edit
Show drafts 
volume_up 
 
 
4. Frontend Development (React App)
Project Setup:
1.	Create a React App:
Bash
cd .. (Navigate to the parent directory)
npx create-react-app todo-list
cd todo-list
Use code with caution.
content_copy
2.	Install Dependencies:
Bash
npm install axios
Use code with caution.
content_copy
o	axios: Promise-based HTTP client for making API calls from frontend to backend.
Component Structure:
1.	Create Components: 
o	Task.js: Reusable component for displaying a single task with title and completion checkbox.
o	TaskList.js: Component to render a list of Task components.
o	AddTask.js: Component with a form for adding new tasks.
o	App.js: Main application component that manages state, fetches tasks from the API, and renders other components.
Example - Task.js:
JavaScript
import React from 'react';

const Task = ({ task, onToggleCompleted }) => (
  <li className={task.completed ? 'completed' : ''}>
    <input type="checkbox" checked={task.completed} onChange={() => onToggleCompleted(task.id)} />
    <span>{task.title}</span>
  </li>
);

export default Task;
Use code with caution.
content_copy
5. API Integration:
1.	Use Axios: 
o	In App.js, use Axios to fetch tasks from the backend API (/todos) and update the application state.
o	Use Axios again to send requests for creating, updating, and deleting tasks based on user actions.
Example - Fetching Tasks:
JavaScript
import React, { useState, useEffect } from 'react';
import axios from 'axios';
import Task from './Task';
import TaskList from './TaskList';
import AddTask from './AddTask';

const App = () => {
  const [tasks, setTasks] = useState([]);
  const [isLoading, setIsLoading] = useState(false);
  const [error, setError] = useState(null);

  useEffect(() => {
    const fetchTasks = async () => {
      setIsLoading(true);
      try {
        const response = await axios.get('/todos');
        setTasks(response.data);
      } catch (err) {
        setError(err.message);
      } finally {
        setIsLoading(false);
      }
    };

    fetchTasks();
  }, []);

  // ... Implement functions for adding, updating, and deleting tasks using Axios ...

  return (
    // ... Render components and handle state ...
  );
};

export default App;
Use code with caution.
content_copy
6. Development Server:
1.	Run the React development server:
Bash
npm start
Use code with caution.
content_copy
2.	This will start the frontend application at http://localhost:3000 (default port). You can view the to-do list interface and test its functionality.
7. Deployment to AWS:
Preparing for Deployment:
1.	Build React App:
Bash
npm run build
Use code with caution.
content_copy
This creates an optimized production build of the React application in the build folder.
2.	Package Backend Code:
o	Consider using a package manager like NPM to create a deployment package for the backend code.
o	This package can include the index.js file, .env file (excluding sensitive information), and any other necessary dependencies.
