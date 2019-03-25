# @gfa

__This project has been discontinued.__

## Original Purpose

This package of libraries was created to assist in quick creation of API endpoints using __Google Cloud HTTP Functions__.

Creating one endpoint involved creating a single function with a small amount of code and reasonable defaults that used __Google Datastore__ or __Firestore__ as databases.

## Why It Has Been Discontinued

__Firebase Functions__ allow developers to deploy a single Express app as one endpoint to handle it all. 

```js
const functions = require('firebase-functions')
const Express = require('express')

var app = new Express()
app.get('/', (req, res) => {
  // .. my code here
})

// add other endpoints, middleware, etc

exports.app = functions.http.onRequest(app)
```

It has all of the advantages of a monolith but deployable to a serverless environment:

* Express is a widely known library
* Fully portable app: if Google Functions doesn't work for you, just deploy your app to a real server
* Better support of other database libraries such as Sequelize
* Allows integration testing

The way the Firebase client handles deploys also suggests the above approach is better. Despite being able to create multiple functions, all of them __will share the same code__. This somewhat beats the purpose of deploying small functions with limited functionality.

During development of this library, I had to rebuild things like routing from the scratch. While I could use the routing library that Express uses, it began to indicate that it was a better approach to just build an Express app already.

Another advantage of deploying a single app function is that it can be deployed on a domain using Firebase Hosting rewrites:

```js
  // in firebase.json
  "rewrites": [
    {"source": "**", "function": "app"}
  ]
```

You can then add a domain such as __api.mydomain.com__ to Firebase Hosting and use the rewrite rule above to point all requests to __`api.mydomain.com/*`__ to your function and it will be routed like an Express app. You can also use the original __cloudfunctions.net__ domain if you want.
