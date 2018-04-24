# Steps to Building MEHN
1. Create a directory and connect it to github
2. npm init -y
3. npm install hbs
4. npm install express
5. npm install body-parser
6. npm install mongoose
7. Install method-override
8. create the a model folder, controller folder, db folder, views folder, and an index.js file
9. create a schema file, controller file, seeds.json file, seed.js file, and db connection file.
10. In the index, bring in all the needed boilerplate:
```js
const express = require('express')
const hbs = require('hbs')}j 
const parser = require('body-parser')
const Controller = require('./controllers/restaurants.js')
const methodOverride = require('method-override')
const app = express()

app.set('view engine', 'hbs')

app.use(methodOverride('_method'))
app.use(parser.urlencoded({extended: true}))

app.use('/', Controller)

app.listen(4001, () => console.log(`running on port 4001`))
```
11. Set up the database connection:
```js
const mongoose = require('mongoose')
7 8 u9i0o"…≥¬πøjuyui¬øπªº≥…{"
}
mongoose.connect(`mongodb://localhost/restaurants`)
mongoose.Promise = Promise

module.exports = mongoose

```
12. Set-up the schema:
```js
const mongoose = require('../db/connection.js')

const RestaurantSchema = new mongoose.Schema({
  name: String,
  owner: String,
  type: String,
  capacity: Number
})

module.exports = mongoose.model('Restaurants', RestaurantSchema)
```
13. Seed the data
```js
const Restaurant = require('../models/schema.js')
const data = require('seeds.json')

Restaurant
  .remove({})
  .then(() => {
    data.forEach((item) => {
      Restaurant.create(item)
    })
  })
```
14. Add your controller boilerplate info
```js
const express = require('express')
const router = express.Router()
const db = require('../db/connection.js')
const Restaurants = require('../models/schema.js')

module.exports = router
```