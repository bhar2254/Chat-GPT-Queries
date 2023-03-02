# Prompt: Tutorial: Create a single page javascript bootstrap web app using node express
## Date: 2 March 2023

To create a single page JavaScript Bootstrap web app using Node Express, you can follow these steps:  
1. Create a new Node.js project:  
```
mkdir my-app
cd my-app
npm init
```
2. Install the required dependencies:  
```
npm install express bootstrap jquery popper.js
```
3. Create an `index.html` file in the root directory of your project and add the following HTML code to it:  
```
<!DOCTYPE html>
<html>
<head>
    <title>My App</title>
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <link rel="stylesheet" href="/node_modules/bootstrap/dist/css/bootstrap.min.css">
</head>
<body>
    <div class="container">
        <h1>Hello World!</h1>
        <p>This is a single page JavaScript Bootstrap web app created using Node Express.</p>
    </div>
    <script src="/node_modules/jquery/dist/jquery.min.js"></script>
    <script src="/node_modules/popper.js/dist/umd/popper.min.js"></script>
    <script src="/node_modules/bootstrap/dist/js/bootstrap.min.js"></script>
</body>
</html>
```
4. Create a new file called `server.js` in the root directory of your project and add the following code to it:  
```
const express = require('express');
const app = express();

app.use(express.static(__dirname + '/public'));

app.get('/', function(req, res) {
    res.sendFile(__dirname + '/index.html');
});

const port = process.env.PORT || 3000;
app.listen(port, function() {
    console.log('Server running at http://localhost:' + port);
});
```
5. Run the app by executing the following command:  
```
node server.js
```
6. Open your browser and navigate to `http://localhost:3000` to see your single page JavaScript Bootstrap web app in action!  
This example assumes that you have installed Node.js and npm on your system. If you haven't, please install them first before proceeding with the steps above.