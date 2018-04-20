# Deployment

## Requirements
* Server
* Services
* Dependencies
* Executable Code
* Configuration

## Heroku
* use ```git push heroku master```
* If you are working off a non-master branch, you will have to use ```git push heroku solution: master```
* sets up a new server for us automatically

## Steps
1. Go to your project directory
2. type ```$ heroku create project-name-here```
3. Go to https://mlab.com/home and create a new MongoDB deployment
4. Choose AWS Sandbox
5. Set the db name to the same as in your connection
6. Click on your db and then go to the Users tab. Users are the admin accounts for the db. 
7. Copy the URI link (substituting your password and username)
8. In your connection, put:
```js
if (process.env.NODE_ENV === "production") {
  mongoose.connect(process.env.MLAB_URL)
} else {
  mongoose.connect(connectionURI)
}
```
9. In bash, set the MLAB_URL
```bash
$ heroku config:set MLAB_URL=mongodb://<your_user_login>:<your_user_password>@ds015760.mlab.com:15760/<your_db_name>
```
mongodb://admin:victory2018@ds251889.mlab.com:51889/2018candidates_db
10. In your index, re-write the app.listen stuff to look like:
```js
app.set('port', process.env.PORT || 3001)

app.listen(app.get('port'), () => {
  console.log(`✅ PORT: ${app.get('port')} 🌟`)
})
```
11. Create a "Procfile" in the root of the project and put in it:
```web: node index.js```
12. Push to heroku: ```$ git push heroku master``` or ```$ git push heroku authentication:master```
13. ```$ heroku run node db/seed.js```
14. ```$ heroku open```