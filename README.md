# MEAN Stack Deployment to Ubuntu in AWS

**MEAN** Stack is a combination of following components:
1. MongoDB (Document database) - Stores and allows to retrieve data.
2. Express (Back-end application framework) - Makes requests to Database for Reads and Writes.
3. Angular (Front-end application framework) - Handles Client and Server Requests
4. Node.js (JavaScript runtime environment) - Accepts requests and displays results to end user

# **Step 0 - Preparing prerequisites**

In order to complete this project you will need an AWS account and a virtual server with Ubuntu Server OS.

If you do not have an AWS account - go back to **[Project 1 Step 0**](https://www.notion.so/PROJECT-1-WEB-STACK-IMPLEMENTATION-6bdce943581e4743a5f1a65b331e567f)** to sign in to AWS free tier account ans create a new EC2 Instance of t2.nano family with Ubuntu Server 20.04 LTS (HVM) image. Remember, you can have multiple EC2 instances, but make sure you **STOP** the ones you are not working with at the moment to save available free hours.

# **Task**

In this assignment you are going to implement a simple Book Register web form using MEAN stack.

# **Step 1: Install NodeJs**

`Node.js` is a JavaScript runtime built on Chrome’s V8 JavaScript engine. `Node.js` is used in this tutorial to set up the Express routes and AngularJS controllers.

Update ubuntu

`sudo apt update`

Upgrade ubuntu

`sudo apt upgrade`

Install NodeJS

`sudo apt install -y nodejs`

# **Step 2: Install MongoDB**

MongoDB stores data in flexible, `JSON-like` documents. Fields in a database can vary from document to document and data structure can be changed over time. For our example application, we are adding book records to MongoDB that contain book name, isbn number, author, and number of pages.

`sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv 0C49F3730359A14518585931BC711F9BA15703C6`

`echo "deb [ arch=amd64 ] https://repo.mongodb.org/apt/ubuntu trusty/mongodb-org/3.4 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-3.4.list`

Install MongoDB

`sudo apt install -y mongodb`

Start The server

`sudo service mongodb start`

Verify that the service is up and running

`sudo systemctl status mongodb`

Install `[npm](https://www.npmjs.com)` - Node package manager.

`sudo apt install -y npm`

Install ‘[body-parser](https://www.npmjs.com/package/body-parser) package

We need ‘body-parser’ package to help us process JSON files passed in requests to the server.

`sudo npm install body-parser`

Create a folder named ‘Books’

`mkdir Books && cd Books`

In the Books directory, Initialize npm project

`npm init`

Add a file to it named server.js

`vi server.js`

Copy and paste the web server code below into the `server.js` file.

`var express = require('express');
var bodyParser = require('body-parser');
var app = express();
app.use(express.static(__dirname + '/public'));
app.use(bodyParser.json());
require('./apps/routes')(app);
app.set('port', 3300);
app.listen(app.get('port'), function() {
    console.log('Server up: http://localhost:' + app.get('port'));
});`

# **Step 3: Install [Express](https://expressjs.com/) and set up routes to the server**

Express is a minimal and flexible `Node.js` web application framework that provides features for web and mobile applications. We will use Express in to pass book information to and from our MongoDB database.

We also will use **[Mongoose](https://mongoosejs.com/)** package which provides a straight-forward, schema-based solution to model your application data. We will use Mongoose to establish a schema for the database to store data of our book register.

`sudo npm install express mongoose`

In ‘Books’ folder, create a folder named `apps`

`mkdir apps && cd apps`

Create a file named `routes.js`

`vi routes.js`

Copy and paste the code below into routes.js

`var Book = require('./models/book');
module.exports = function(app) {
  app.get('/book', function(req, res) {
    Book.find({}, function(err, result) {
      **if** ( err ) throw err;
      res.json(result);
    });
  }); 
  app.post('/book', function(req, res) {
    var book = new Book( {
      name:req.body.name,
      isbn:req.body.isbn,
      author:req.body.author,
      pages:req.body.pages
    });
    book.save(function(err, result) {
      **if** ( err ) throw err;
      res.json( {
        message:"Successfully added book",
        book:result
      });
    });
  });
  app.delete("/book/:isbn", function(req, res) {
    Book.findOneAndRemove(req.query, function(err, result) {
      **if** ( err ) throw err;
      res.json( {
        message: "Successfully deleted the book",
        book: result
      });
    });
  });
  var path = require('path');
  app.get('*', function(req, res) {
    res.sendfile(path.join(__dirname + '/public', 'index.html'));
  });
};`

In the ‘apps’ folder, create a folder named `models`

`mkdir models && cd models`

Create a file named `book.js`

`vi book.js`

Copy and paste the code below into ‘book.js’

`var mongoose = require('mongoose');
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
module.exports = mongoose.model('Book', bookSchema);`