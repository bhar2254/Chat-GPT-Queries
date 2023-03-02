# Prompt: Tutorial: Build a Basic CRUD App with Node.js
## Date: 2 March 2023

Creating a basic CRUD (Create, Read, Update, Delete) application with Node.js is a great way to get started with building web applications. In this tutorial, we'll walk through the steps to build a simple application that allows users to create, read, update, and delete items from a list.  
To build this app, we'll be using the following tools and technologies:  
- Node.js: a JavaScript runtime that allows us to run JavaScript code on the server-side  
- Express: a web application framework for Node.js  
- MongoDB: a NoSQL database that we'll use to store our data  
- Mongoose: a MongoDB object modeling tool that allows us to interact with the database using JavaScript objects  

Let's get started!  

## Step 1: Set up the project
First, we'll create a new directory for our project and initialize a new Node.js project using the following commands in your terminal:  
```
mkdir node-crud-app
cd node-crud-app
npm init -y
```
This will create a new directory called node-crud-app, navigate to that directory, and initialize a new Node.js project with default settings.  
Next, we'll install the dependencies we need for our application. Run the following command in your terminal:  
```
npm install express mongoose body-parser
```
This will install Express, Mongoose, and the Body Parser middleware, which we'll use to parse incoming request bodies.  

## Step 2: Create the server and database connection  
Next, we'll create a new file called index.js and add the following code:  
```
const express = require('express');
const mongoose = require('mongoose');
const bodyParser = require('body-parser');

const app = express();

// Connect to the database
mongoose.connect('mongodb://localhost/node-crud-app', {
  useNewUrlParser: true,
  useUnifiedTopology: true,
});

// Define a schema for our data
const itemSchema = new mongoose.Schema({
  name: String,
  description: String,
});

// Create a model based on the schema
const Item = mongoose.model('Item', itemSchema);

// Set up middleware
app.use(bodyParser.urlencoded({ extended: false }));

// Start the server
const port = process.env.PORT || 3000;
app.listen(port, () => {
  console.log(`Server started on port ${port}`);
});
```

This code creates a new Express application, connects to a MongoDB database named `node-crud-app`, defines a schema for our data, creates a Mongoose model based on the schema, and starts the server listening on port 3000.  

## Step 3: Add routes for CRUD operations  
Now that we have our server and database set up, we can start adding routes to handle the CRUD operations.  

### Read (GET) operation  
Let's start by adding a route to retrieve all items from the database. Add the following code to your index.js file:  
```
// Get all items
app.get('/items', async (req, res) => {
  const items = await Item.find();
  res.send(items);
});
```

This code creates a new route at the URL /items that retrieves all items from the database using the Item.find() method and sends the result back as a response.  
### Create (POST) operation
Next, let's add a route to create a new item in the database. Add the following code to your `index.js` file:
```
// Create a new item
app.post('/items', async (req, res) => {
  const { name, description } = req.body;
  const item = new Item({ name, description });
  await item.save();
  res.send(item);
});
```
This code creates a new route at the
