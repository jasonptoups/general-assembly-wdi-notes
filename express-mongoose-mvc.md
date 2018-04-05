# MongoDB
## Why use a Database?
* Permanence: once it’s in the database, unless we delete it, it’s there. Production databases have data all over
* Speed: much faster than reading from a file
* Consistent: rows and columns. Very consistent. (more so for SQL)
* Scalable: can handle many requests at once
* Searchable: can query, sort, filter, etc

## Relational vs Non-Relational
* __Relational__: data sorted into columns and rows. SQL is an example
  * Better when data is consistent across records
* __Non-relational__: data is less structured. MongoDB is an example
  * Better when data is not super consistent
  * Medical records are usually in a non-relational database

## MongoDB
* Document database
  * High performance
  * High availability
  * Automatic Scaling
* Basically a document with a bunch of objects (key: value pairs)

## Terminology
* Database: Actual set of data being stored
* Database Management System: A system for managing a database, like MongoDB
* Database CLI: Mongo has its own command line

## A Document
* A data-structure with key-value pairs
* Stored as BSON
* can support all data types
* A document is analogous to a row in a table
* Documents can contain sub documents (like objects within a key value pair)

```json
{
   "_id" : ObjectId("54c955492b7c8eb21818bd09"),
   "address" : {
      "street" : "2 Avenue",
      "zipcode" : "10075",
      "building" : "1480",
      "coord" : [ -73.9557413, 40.7720266 ],
   }
}
```

## A Collection
* A bunch of documents, each one a single whole record. MongoDB stores documents in collections
* Documents in a collection __must have a unique ```_id``` field__

## Examples:
## MongoDB in the CLI
```bash
$ mongo
> help
> show dbs
// this will show the databases you have
> use restaurant_db
// this will connect us to the database or create it
> db
// => restaurant_db
// once we add a document to this database, it will be visible in the full list
> db.restaurants.insert({
... "name": "&pizza",
... "address": {
... "street": "1400 K st NW",
... "zipcode": 20005,
... "city": "Washington",
... "state": "DC"
... },
... "url": "https://andpizza.com/"
... })
// this creates a collection "restaurants" and created a document for &pizza
> db.restaurants.find()
// this shows all the documents you have in that collection with a created object id
```
* When we use Mongo, we insert documents into collections. So first create a collection

## Insert, Find, Remove, 
### Insert: 
```json
db.collectionname.insert({object here})
```
Insert can be done by copying and pasting an array from your text editor

### Find: 
```json
> db.restaurants.find().pretty()
> db.restaurants.find({"name": "Maydan"})
> db.restaurants.find({"address.zipcode": 20001})
```

### Update:
```bash
> db.your_collection.update(
  { criteria },
  {
    $set: { assignments }
  },
  { options }
)
// syntax above
> db.restaurants.update({"name": "Maydan"}, { $set: {"rating": "5"} 
})
// First argument is the search, second argument has a $set operator with a new key value pair following it.
> db.restaurants.update({"address.zipcode": 20001}, { $set: {"city": "Washington"} }, {multi: true})
// The last argument defaults to false, so it stops after the first match. 
```

### Remove: 
```json
> db.restaurants.remove({"name": "Red Derby"})
```  
This will remove the record with the name of "Red Derby"

## Review
* MongoDB is very performant
* MongoDB can handle a lot of transactions
* Can handle sub documents very well. Nesting is allowed


# Express & Mongoose
## Objectives
* Learn MVC architecture
* Using Express, Mongo, Mongoose, Handlebars

## MVC: Model, View, and Controller
Separate by use instead of by language. It's all in Javascript
1. __Model__: describe the feature
2. __View__: present the data for a feature
3. __Controller__: compose the data and its presentation through some business logic. Switch board. 

### Model
* Use the model to interact with data. And we use the model to structure our data

### The View
* This is what the user sees and interacts with (with HTML and CSS that gets rendered in the browser)
* Each model can have a few views. Typical layout:
  * Index: show every instance of a model (e.g., all items for sale)
  * Show: see a single instance of a model (e.g., single item for sale)
  * Edit: update a single instance of a model (e.g., edit the item)
  * New: create a new instance of a model (e.g., add a new item for sale)

### The Controller
* Where we bring models and views together, using "business logic"
* Tell the model we want to store new data based on a request has been made. E.g., adding a new item for sale
 
### Example
1. Browser: Type a URL into the browser
2. Routing: User goes to the index page or some other page
3. Controller function: Tells the computer to go to the model to fetch data
4. Model: Gets data out of the database
5. Database: responds to data request
6. Model responds to Controller
7. Controller sends that data to a View to show it in a certain way
8. View fills in the data using Handlebars
9. View can be modified by CSS, javascript in it, etc.
10. Responds to the user

## Mongoose
Object Document Mapper: a tool to map between native objects and documents stored in a document store (like MongoDB)
* Transforms documents to objects
* We could have used MongoDB javascript library, but Mongoose is easier

## CRUD
We will use CRUD to build out routes for working with our data. The platform needs to handle the full CRUD spectrum. 
- Create: 
- Read: 
- Update: 
- Delete: 

## REST
HTTP requests have a **path** (the text after the domain name) and a method. Methods:
* GET: Read method. Retrieves data (/todos or /todos/1)
* POST: Create method. Adds data (/todos)
* PUT: Update method. Modify data (/todos/1)
* DELETE: Delete method. Remove data. (/todos/1)

## Practice
In Bash:
```bash
$ mkdir express-mvc-todo
$ cd express-mvc-todo
$ npm init -y
$ npm install express
$ mkdir db models views controllers
$ npm install mongoose
$ touch db/connection.js
$ touch models/todo.js
$ touch db/seeds.js
```
In connection.js:
```js
const mongoose = require('mongoose')
mongoose.connect('mongodb://localhost/todo')
// Not sure why we made this the one we're focused on
mongoose.Promise = Promise
// Not sure what this is but James is talking about it...
module.exports = mongoose
// here we export mongoose so it can be used elsewhere
```
In todo.js
```js
const mongoose = require('./connection')
// need the ./ because otherwise it would grab a node module
const ToDoSchema = new mongoose.Schema({
  title: String,
  complete: Boolean
})
// This is our blueprint for our to-do items. Each will have a title and whether it's complete. 
const Todo = mongoose.model('Todo', ToDoSchema)
// creates a variable name for a collection in our todo database. Applies the schema we declared
module.exports = Todo
```
In seeds.js:
```js
const mongoose = require('../models/todo')
const thingsToDo = [
  {
    title: 'Make it through Express week',
    complete: false
  },
  {
    title: 'Complete unit 1',
    complete: true
  }
]
Todo
  .remove({})
  .then(() => {
    return Todo.collection.insert(thingsToDo)
    // creates a collection based on the inserted data
    // adding the return allows us to console.log it later
    //
  })
  .then(() => {
    process.exit()
  })
  // these are chained methods. They run in succession
```
After all that, in bash:
```bash
node seeds.js
```
* If we want to do something with a response from a database, we need to put it in a Promise. Databases take a moment to run. Javascript could outpace it. 
* Promises are a set of methods that wait to let the database to operate. ```Then``` is a good example. ```catch``` is another and is used for error handling.
Now, in bash:
```bash
$ touch index.js
$ touch controllers/todos.js
```
In index.js, establish the boilerplate:
```js
const express = require('express')
// be sure to install first
const app = express()
const hbs = require('hbs')
// be sure to install first
const todosController = require('./controllers/todos')
app.set('view engine', 'hbs')
// This is what lets handlebars work
app.use('/', todosController)
// This tells the app to use the todosController

app.listen(4004, () => console.log('running on port 4004'))
// This is where our node express app will run
```
## Routes
We will use "routes" to define the different things a user can do on our website

### Index Route
In this route, we will show all the to do items. 
In the Controller todos.js:
```js
const router = require('express').Router()
const Todo = require('../models/todo')
// now we're establishing router as the main switching. We had previously used something else. App I think or something.
// Todo is our link to the data

router.get('/', (req, res) => {
  Todo
    .find({})
    // Find all the Todos
    .then(todos => {
      res.render('index.hbs', { todos: todos })
    })
    // This is saying render the index.hbs view and pass to it the todos variable. 
})
```

### Show Route
In this route, we will show just one to-do
In the Controller todos.js:
```js
router.get('/:id', (req, res) => {
  Todo
    .findOne({ _id: req.params.id })
    .then( todo => {
      res.render('show', { todo })
    })
})
```
In the views folder, create a new file called show.hbs
```html
<h1> {{ todo.title }} </h1>
```

### Create Route
In this route, we will be able to add a to-do
First, install body-parser
In the Controller todos.js
```js
router.get('/new', (req, res) => {
  res.render('new')
})
```
In the index.js file, we have to bring in body parser:
```js
const parser = require('body-parser')
app.use(parser.urlencoded({ extended: true}))
// this has to be above the controller
```
We have to create a router for the post '/' route first. In the controller:
```js
router.post('/', (req, res) => {
  Todo.create(req.body)
  // .create is a mongoose method
  // req.body is just a method from body-parser
    .then(todo => {
      res.redirect('/')
      // res.redirect resets the request response cycle all over again. res.render does not.
    })
})
```
In our new.hbs file, we need to create a form:
```html
<h1>New To Do</h1>
<form action="/" method="post">
  <label for="">Title:</label>
  // This appears to the left
  <input type="text" name="title">
  // this will be the text input field. "title" has to match our schema. 
  <input type="submit" value="Create">
  // this has to be a submit. Automatically works with 'enter'
</form>
```
### Put Route (Update Route)
HTML does not have a native update route, the way the form has post and get. So we have to install a package
In bash:
```bash
$ npm install method-override
```
In our index.js file, we have to install it:
```js
const methodOverride = require('method-override')
app.use(methodOverride('_method'))
// this defines the method we're using on the URLs. Has to be above controller
```
In our controller, add a route to get the edit/id page, which will render a form. And we need to add a route for the put on the id page:
```js
router.get('/edit/:id', (req, res) => {
  // we're writing what the /edit/:id page should look like
  Todo
    .findById(req.params.id)
    // Grab the db item that matches the id
    .then(todo => {
      res.render('edit', { todo })
    })
    // render the edit page
})
router.put('/:id', (req, res) => {
  Todo.findOneAndUpdate(
    {_id: req.params.id}, 
    // the first argument is what to search for
    {
      title: req.body.title,
      complete: req.body.complete === 'on'
    }, 
    // the second argument is what we're updating using body parser
    {new: true}
    )
    // the third argument tells our database to update the entire record with this new information and record it as a new record
    .then(todo => {
      res.redirect('/')
    })
    // then, we redirect to the homepage
})
```
We have to update the Edit view so it has a form
```js
<h2>Edit To Do:</h2>
<form action="/{{todo._id}}?_method=put" method="post">
  <label>Title:</label>
  <input type="text" name="title" value="{{todo.title}}">
  <label>Complete:</label>
  {{#if todo.complete}}
    <input type="checkbox" name="complete" checked>
  {{else}}
    <input type="checkbox" name="complete">
  {{/if}}
  <input type="submit" value="Update">
</form>
{{! // Use a post form and then modify it to be a put
// ?_method=put is telling our middleware
// /{{todo.id}} says we're doing a put method on the unique id (sort of like the show method)
// the ? and & gives us optional parameters. We don't want a bunch of slashes. 
// Adding "checked" allows it to show as checked --}}
```

### Methods
res.render: Shows a view
res.send: 
res.redirect: Resets the request response cycle all over again.