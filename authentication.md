# Authentication

## Passport
Passport is a middleware for Node. It helps us authenticate requests.  
Why Use Passport?
* It's encrypted
* It is open-source

### Strategies
Strategies are different ways of logging in. You make have to make your own. Or maybe you go off Twitter or Facebook. Or you can do local.  

__Note__: there will be a lot of boilerplate code in this. You would probably just get this from the documentation. 

### Encryption
__Hashing__: Takes a string and converts it to another string of specified length in a way where it cannot be converted back. To use this with a password, hash the password a user creates and then when they try to log in, hash the attempt and compare if it matches the stored version.  

__Encryption__: The major difference is that encryption is meant to be reversible if you have the right key. The recipient and the sender need keys. In symmetric encryption, the keys are the same on both ends. On public key encryption, the sender uses one key and the recipient uses another one. 

### Sessions/Flash
We use Sessions to determine if someone is logged in and show them a certain page. Sessions in JS are an object. The object allows us to track information across pages. Sessions are used mostly for user authentication and authorization, shopping carts, and setting a current model to persist throughout requests.  

Flash is a message that is generated for a specific user. It only lasts for 1 request. 

## Setting up Passport
### Model
Have a user model:
```js
var mongoose = require('../db/connection')
var bcrypt = require('bcrypt-nodejs')

var User = mongoose.Schema({
  local: {
    email: String,
    password: String
  }
})

module.exports = mongoose.model('User', User)
```

### Set up the app.js file
Do your typical boilerplate. Then be sure to have this:
```js
const flash = require('connect-flash')

app.use(session({ secret: 'WDI-GENERAL-ASSEMBLY-EXPRESS' }))
app.use(flash())

// this needs to be below the session name 
const passport = require('passport')
const passportConfig = require('./config/passport')
passportConfig(passport)
app.use(passport.initialize())
// starts it up
app.use(passport.session())
// allows us to use sessions
```

### Improve the model:
We need to add bcrypt to it and create a done() method:
```js
var mongoose = require('../db/connection')
var bcrypt = require('bcrypt-nodejs')

var User = mongoose.Schema({
  local: {
    email: String,
    password: String
  }
})

User.methods.encrypt = function (password) {
  // we are setting up the encryption method for passwords
  return bcrypt.hashSync(password, bcrypt.genSaltSync(8), null)
}

module.exports = mongoose.model('User', User)
```

### Now go to config/passport.js
The first thing we're going to do is import the local strategy. We could also do Facebook or other strategies.  
```js
const LocalStrategy = require('passport-local').Strategy
const User = require('../models/user')
// bring in the User model
module.exports = function (passport) {
  // need to accept 1 argument bc we pass in a variable in the index

  passport.serializeUser(function (user, done) {
    done(null, user.id)
  })

  passport.deserializeUser(function (id, done) {
    User.findById(id, function (err, user) {
      done(err, user)
    })
  })

  // this is our sign-up logic
  passport.use('local-strategy', new LocalStrategy({
    usernameField: 'email',
    passwordField: 'password',
    passReqToCallback: true
  }, function(req, email, password, done) {
    User.findOne({'local.email': email}, function (err, user) {
      if (err) return done(err)
      if (user) {
        return done(null, false, req.flash('signupMessage', 'That email adddress is already in use'))
        // checks if the email address already exists, if so, return that it's in use
      } else {
        const newUser = new User()
        // create new User 
        newUser.local.email = email
        newUser.local.password = newUser.encrypt(password)
        // hash the password
        newUser.save(err => {
          if (err) throw err
          // throw is like a return for an error. It exits here. 
          return done(null, newUser)
        })
      }
    })
  }))
}
```

### Routes
We will need a POST for log-in and sign-up. 
```js
var express = require('express')
var router = express.Router()
var bodyParser = require('body-parser')
var methodOverride = require('method-override')
var passport = require('passport')

router.post('/signup', (req, res) => {
  const signupStrategy = passport.authenticate('local-signup', {
    // this is some object that works with passport. The failure will show the flash we defined in our passport config. 
    successRedirect: '/',
    failureRedirect: '/signup',
    failureFlash: true
  })
  return signupStrategy(req, res)
})

module.exports = router
```

### Fixing our Flash
In the app.js:
```js
const flash = require('connect-flash')
const cookieParser = require('cookie-parser')
const session = require('express-session')

app.use(cookieParser())
app.use(session({ secret: 'WDI-GENERAL-ASSEMBLY-EXPRESS' }))
app.use(flash())
```

In the controller:
```js
router.get('/signup', (req, res) => {
  res.render('signup', {
    message: req.flash('signupMessage')
  })
})
```

In the signup view:
```js
{{#if message }}
  <div class="alert alert-danger">{{message}}</div>
  {{/if}}
<h2>Signup</h2>
// If using API, would have to add a message to the JSON file.
```

## Setting up the Login logic
### Add to the config/passport.js
```js
  // This is the logic for logging in. It goes under the export long series of functions
  passport.use('local-login', new LocalStrategy({
    usernameField: 'email',
    passwordField: 'password',
    passReqToCallback: true
  }, function (req, email, password, done) {
    User.findOne({'local.email': email}, function(err, user) {
      if (err) return done(err)
      if (!user) {
        return done(null, false, req.flash('loginMessage', 'No user found for that email address. Please use the sign-up link or try again'))
      }
      if (!user.validPassword(password)) {
        return done(null, false, req.flash('loginMessage', 'Wrong password'))
      }
      return done(null, user)
    })
  }))
}
```

### Update the model with a new method
```js
User.methods.validPassword = function(password) {
  return bcrypt.compareSync(password, this.local.password)
}
```

### Add to the Login View:
```html
{{#if message }}
  <div class="alert alert-danger">{{message}}</div>
{{/if}}
```

### Update the controller with the GET and POST methods for /login
```js
router.get('/login', (req, res) => {
  res.render('login', {
    message: req.flash('loginMessage')
  })
})

router.post('/login', (req, res) => {
  const loginStrategy = passport.authenticate('local-login', {
    successRedirect: '/',
    failureRedirect: '/login',
    failureFlash: true
  })
  return loginStrategy(req, res)
})
```

## Setting up the Sessions
### Fix the app.js
```js
// Place this right before the controller. The next() invokes the next piece of middleware
app.use(session({ secret: 'WDI-GENERAL-ASSEMBLY-EXPRESS' }))

app.use(function (req, res, next) {
  res.locals.currentUser = req.user
  next()
})
```

### Update the layout to make it appear as though you are logged in:
```html
<div class="collapse navbar-collapse" id="user-menu">
  <ul class="nav navbar-nav">
    {{#if currentUser}}
    <li><a href='/logout'>Logout {{currentUser.local.email}}</a></li>
    {{else}}
    <li><a href="/login">Login</a></li>
    <li><a href="/signup">Signup</a></li>
    {{/if}}
    <li><a href="/secret">Secret page (only if authenticated)</a></li>
  </ul>
</div><!-- /.navbar-collapse -->
```

### Update the controller
```js
router.get('/logout', (req, res) => {
  req.logout()
  res.direct('/')
})
```

### Set-up a get for the secret page
In the router:
```js
router.get('/secret', (req, res) => {
  (req.isAuthenticated()) ? res.render('secret') : res.redirect('/')
})
```