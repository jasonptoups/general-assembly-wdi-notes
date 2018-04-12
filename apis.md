# Bringing in APIs and Ajax
## What is an API?
So far, we have been rendering static pages that only change when the page refreshes. It would still only change if we changed the data on the backend.  
We can use APIs to access third-party data. We can also use APIs to make our page dynamic. We can pass through variables to the front end. 
* __API__: Application Programing Interface. Accessing the DOM is a form of API. Accessing third party data is an API.  
* __XML__: eXtensible Markup Language. It is serialized data based on HTML. It's fat, ugly, and cumbersome.  
* __JSON__: More modern data format. Stands for JavaScript Object Notation. Always use this if possible.  
* __Serialization__: A way of encoding structured data (objects, arrays) into strings of text.  

## Working with APIs
* __Public or Private__: Public are accessible to anyone. Private requires special access. Most are mixed, which allow public view but limited edit.

## Intro to AJAX
__AJAX__: Asynchronous JavaScript and XML. AJAX is a way to update changed data without reloading the whole page. 
* We use AJAX with fetch functions. We can do all of the Get, Post, Put, Patch, and Delete functions
```js
const url = 'http://pokeapi.co/api/v2/pokemon/'
// start by defining the source of the data

fetch(url)
// this is the first step of starting a fetch. It will go to the URL. It will return a stream
  .then(stream => stream.json())
  // here we are telling that stream to be a json() file. It has to be implicitly returned. 
  .then(res => console.log(res))
  // We are now console logging that response. 
  .catch(err => console.error(err))
  // If the above is rejected, show the error.
```
Another, more detailed version:
```js
fetch(url)
  .then(res => {
    console.log('success!', res)
    // show the response if it works. It has a lot of data
    return res.json()
    // convert that response to json
  })
  .catch((error) => {
    console.log('error:', error)
    // show an error if there's an error
  })
  .then(res => console.log(res.name))
  // take the json object, pull just the name (squirtle) and display it.

```

## Promises
```.then()```: runs if the Promise is fulfilled  
```.catch()```: runs if the Promise is rejected  
```.finally()```: Always runs, whether the Promise is rejected or fulfilled  
* The input in these promises is always the return from the previous function

## Get API
```js
const url = 'https://tunr-api.herokuapp.com/artists/'

get.addEventListener('click', () => {
  console.log('GET clicked!')
  fetch(url).then(res => res.json())
    .then(res => console.log(res))
    .catch(err => console.error(err))
})
    // this will console log the API
```

## Post API
```js
  fetch(url, {
    method: 'POST',
    // what we're doing
    headers: {
      'Content-Type': 'application/json'
    },
    // tells the computer what data to expect
    body: JSON.stringify({
     artist: {
       name: 'Beyonce',
       nationality: 'USA',
       photo_url: 'http://www.placebear.com/200/200'
     }
    })
  })
    .then(res => res.json())
    .then(res => console.log(res))
```

## Put API
```js
fetch(`${url}/14`, {
  method: 'PUT',
  // what we're doing
  headers: {
    'Content-Type': 'application/json'
  },
  // tell the computer what kind of data it will be getting back
  body: JSON.stringify({
    artist: {
      name: 'HAIM',
      nationality: 'Israel'
    }
  })
})
  .then(res => res.json())
  .then(res => console.log(res))
  .catch(err => console.error(err))
```

## Delete API
```js
destroy.addEventListener('click', () => {
  console.log('DELETE clicked!')
  fetch(url + '/19', {
    method: 'DELETE'
  })
    .then(res => console.log(res))
    .catch(err => console.error(err))
})
```

# Express APIs
## MVC review
* __Model__: Encapsulates information from the database and lets us query it and change data
* __Views__: User interface. Defines how the page renders
* __Controllers__: Defines how routes are handled

## Setting up APIs in your website
Aside on models. If you export the model like this::
```js
mongoose.model('Bookmark', BookmarkSchema)
module.exports = mongoose
```
Then you have you do stuff in the controller like this:
```js
const mongoose = require('./models/schema.js')
const Bookmarks = mongoose.model('Bookmarks')
```

### Get APIs
In the controller:
```js
router.get('/', (req, res) => {
  Bookmark
    .find({})
    .then(bookmarks => res.json(bookmarks))
    // this is really the only change. You're just responding by sending back a json object
    .catch(err => console.error(err))
})
```

### POST to API
In the controller:
```js
router.post('/', (req, res) => {
  Bookmark
    .create(req.body)
    .then(bookmarks => res.json(bookmarks))
})
```
In the index, bring in body parser, but tell it to parse json
```js
const parser = require('body-parser')
app.use(parser.json())
```
Then, you can use Postman if you want. It just makes it easier to not have to make an html form and javascript, etc:
1. Open Postman
2. Change the dropdown from Get to Post
3. Update the headers to add: ```"Content-Type": "application/json"```
4. Click Body and change it to raw
5. Enter a new object that you intend to insert. E.g.,: 
```js
{
	"title": "example dot com",
	"url": "http://www.example.com"
}
```
* We can use PostMan to make post and other requests to the database to quickly make changes in it. 
* But to do so, you need to define a post route in the controller. 

### PUT to API
In the controller: 
```js
router.put('/:title', (req, res) => {
  Bookmark
    .findOneAndUpdate({title: req.params.title}, req.body, {new:true})
    // the first argument is what to search for, the second is what to replace with, third is optional. Says to return new object instead of old.
    .then(bookmark => res.json(bookmark))
})
```
This will only update the fields you list. It won't affect the fields you don't mention.


### DELETE
In the controller, add a delete route. This will return the full list:
```js
router.delete('/:title', (req, res) => {
  Bookmark
    .findOneAndRemove({title: req.params.title})
    .then(() => {
      Bookmark
        .find()
        .then(all => res.send(all))
    })
})
```

## Connecting front and back-end
