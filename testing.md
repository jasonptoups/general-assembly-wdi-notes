# Testing
## Why Use
* When you have large code bases with many features, you risk breaking an old feature when you add a new one. Testing helps us to prevent that and check for breakages without having to test the entire application and all of its dependencies

## Types of Tests
* Execution
  * Manual: refresh the page and see what happens
  * Automated: use scripts
* Granularity
  * Unit: Testing individual functions and methods for particular results
  * Integration: Testing a set of components
  * End to End: Testing each step of a workflow
* Purpose
  * Positive testing: making sure it works when you want it to work
  * Negative testing: making sure it doesn't work when you want it to, like showing errors, etc
  * Load testing: How many people can be on a page at a time
  * Security
  * Usability
  * Compatability
  * User Acceptance Testing

## Test Driven development
A development methodology of writing the tests first, then writing the code to make those tests pass. Thus the process is:
1. Define a test set for the unit
2. Implement the unit
3. Verify that the implementation of the unit makes the tests succeed
4. Refactor
5. Repeat

## Behavior driven development
Not really sure...

## Chai, Mocha, Supertest
WTF are these?
```bash
npm i chai --save-dev
// this will make sure you only have this dependency on the development side
npm i mocha --save-dev
npm i supertest --save-dev

mkdir test
touch test/candies.test.js
```
* Create a test folder
* Save tests with the .test.js file extension
Go to that candies.test.js file: 
```js
const should = require('chai').should()
const expect = require('chai').expect
const supertest = require('supertest')
const api = supertest('http://localhost:3000')
// This is where our app is running

// This test will test whether the page works:
describe("GET /candies", function () {
  it('should return a 200 response', function (done) {
    // here we had named it
    api.get('/candies')
    // get the /candies path
      .set('Accept', 'application/json')
      // the data coming back will be json format
      .expect(200, done)
      // expect a 200 code. 
      // Done is some callback function that once again is not transparent...
  })

  // This tests whether the response is an array
  it('should return an array', function (done) {
    api.get('/candies')
      .set('Accept', 'application/json')
      .end(function (error, response) {
        if (error) return error
        expect(response.body).to.be.an('array')
        done()
      })
  })

  it(`should return an array of objects with 'name' field`, function (done) {
    api.get('/candies').set('Accept', 'application/json').end(function (error, response) {
      if (error) return error
      expect(response.body[0]).to.have.property('name')
      done()
    })
  })

  // alternative to the above:
  it(`should return an array of objects with 'name' field`, function (done) {
    api.get('/candies').set('Accept', 'application/json').end(function (error, response) {
      if (error) return error
      expect(response.body.every(candy => candy.name))
      done()
    })
  })
})
```
