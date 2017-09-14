# Firebase NodeJS Starter

As javascript developers we are always looking to running our NodeJS apps on the server. You have to install nginx to write reverse proxy or any other libraries, configurations for security performance etc. Then what about using firebase hosting features to running your app? Yes it's possible! With Cloud Functions of Firebase, you are able to use power of Google Cloud Services without any extra payment for SSL, security, performance or any other similar topics.

Here I created this repository to help who wants to write NodeJS apps and deploy it easily without thinking backend parts. You can use this repository for Server Side Rendering with ExpressJS or API server. Ok Lets start!

## Before Using It:
I assume you've already installed NodeJS (Version 6+), Firebase Tools, and Git on the machine.

## How To Use It?
- First, you have to create a new Firebase app from Firebase Console.
- After creating app, open your terminal and clone this library with these command:
```sh
$ git clone https://github.com/serdaraglamis/firebase-nodejs-starter ./
```
  - Then we have to install our packages
```sh
$ cd functions
$ npm install
```
- After installed all necessary packages. Make this project your own and connect with your created project by typing:
```sh
$ firebase use --add
```
  - Its ready you can test it locally by typing  (With live reload feature. Thanks to firebase !)
```sh
$ firebase server --only functions,hosting
```
- Also You can deploy project to live url with typing (It will give your url :) )
```sh
$ firebase deploy
```

# How Its Working?
This is an easy Express app using Handlebars to render templates:
```js

const functions = require('firebase-functions'); // This is provided by firebase to use its functions
const express = require('express'); // This is standart express import
const firebase = require('firebase-admin'); // Firebase Admin SDK. You can reach all of your data with easily
const engines = require('consolidate'); // One place for express engines :)
const Handlebars = require('handlebars'); // Template viewing engine
const fs = require('fs'); // Used to read directory listings

// This is for initialize express app
const app = express();

// this is for initalize firebase
const firebaseApp = firebase.initializeApp(functions.config().firebase);

// App view engine setups
app.engine('hbs', engines.handlebars);
app.set('views', './views');
app.set('view engine', 'hbs');
Handlebars.registerPartial('partial', fs.readFileSync(__dirname + '/views/partials/partial.hbs', 'utf8'));

// Express Route - Renders index page
app.get('/', (request, response) => {
    // response.set('Cache-Control', 'public, max-age=300, s-maxage=600'); For caching use that
        response.render('index', {data: {
            author: 'SERDAR AGLAMIS'
        }})
});

// Exports app for firebase https requests
exports.app = functions.https.onRequest(app);
```

### Tech

This project uses a number of open source projects to work properly:

* [Express] - Fast, unopinionated, minimalist web framework for Node.js
* [Consolidate] - Template engine consolidation library for node.js
* [node.js] - Node.js® is a JavaScript runtime built on Chrome's V8 JavaScript engine.
* [Handlebars](http://handlebarsjs.com/) - Minimal Templating on Steroids

And of course this project itself is open source :)

   [node.js]: <http://nodejs.org>
   [express]: <http://expressjs.com>
   [Consolidate]: <https://github.com/tj/consolidate.js/>
