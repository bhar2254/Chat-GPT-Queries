# Prompt: Tutorial: Build a Basic CRUD App with Node.js SQLite Bootstrap
## Date: 2 March 2023

In this tutorial, we will build a simple CRUD (Create, Read, Update, Delete) application using Node.js, SQLite, and Bootstrap.  
### Step 1: Set up the project  
First, we need to set up a new Node.js project. Open your terminal and enter the following commands:  
```
mkdir node-sqlite-crud
cd node-sqlite-crud
npm init -y
```
Next, we'll install the dependencies we need for our application. Run the following command in your terminal:  
```
npm install express sqlite3 ejs
```
This will install Express, SQLite3, and EJS, which we'll use to render HTML templates.  
### Step 2: Create the database and table  
We'll be using SQLite3 as our database for this application. SQLite is a lightweight relational database management system that can be embedded into applications. It's perfect for small to medium-sized applications like ours.  
First, we need to create a new SQLite database file. Run the following command in your terminal:  
```
touch database.sqlite
```
Next, we'll create a new table in our database to store our data. Create a new file called `schema.sql` and add the following SQL code:  
```
CREATE TABLE IF NOT EXISTS items (
  id INTEGER PRIMARY KEY AUTOINCREMENT,
  name TEXT NOT NULL,
  description TEXT NOT NULL
);
```
This code creates a new table called `items` with three columns: `id`, `name`, and `description`. The `id` column is the primary key and will auto-increment with each new row.  
Now, let's create a new Node.js file called `index.js` and add the following code:  
```
const express = require('express');
const sqlite3 = require('sqlite3');
const bodyParser = require('body-parser');
const ejs = require('ejs');

const app = express();
const db = new sqlite3.Database('database.sqlite');

// Set up middleware
app.use(bodyParser.urlencoded({ extended: false }));
app.use(express.static('public'));
app.set('view engine', 'ejs');

// Define routes
app.get('/', (req, res) => {
  db.all('SELECT * FROM items', (err, rows) => {
    if (err) {
      console.log(err);
      return;
    }
    res.render('index', { items: rows });
  });
});

app.get('/items/new', (req, res) => {
  res.render('new');
});

app.post('/items', (req, res) => {
  const { name, description } = req.body;
  db.run('INSERT INTO items (name, description) VALUES (?, ?)', [name, description], (err) => {
    if (err) {
      console.log(err);
      return;
    }
    res.redirect('/');
  });
});

app.get('/items/:id/edit', (req, res) => {
  const { id } = req.params;
  db.get('SELECT * FROM items WHERE id = ?', [id], (err, row) => {
    if (err) {
      console.log(err);
      return;
    }
    res.render('edit', { item: row });
  });
});

app.post('/items/:id', (req, res) => {
  const { name, description } = req.body;
  const { id } = req.params;
  db.run('UPDATE items SET name = ?, description = ? WHERE id = ?', [name, description, id], (err) => {
    if (err) {
      console.log(err);
      return;
    }
    res.redirect('/');
  });
});

app.post('/items/:id/delete', (req, res) => {
  const { id } = req.params
```