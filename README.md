OpenAPI Express middleware
==========================

Use OpenAPI specification to secure, parse, validate, and deserialize requests using an Express middleware

Installation and Use
--------------------
Install using [NPM](https://docs.npmjs.com/getting-started/what-is-npm).

```bash
npm install swagger-express-middleware
```
Then use it in your [Node.js](http://nodejs.org/) script like this:

```javascript
const express = require('express');
const middleware = require('swagger-express-middleware');
const Sequelize = require('sequelize');

const app = express();

const openApiMiddleware = middleware('./path/to/api-spec.yaml');
const sequelize = new Sequelize('postgres://user:pass@example.com:5432/dbname');

app.use(openApiMiddleware({
  metadata: true,
  validate: true,
  parse: true,
  // to using sequelize parsing
  sequelize: sequelize   // | false
}));

// reference the endpoint identified by operation ID
app.createMember(function (req, res, next) {  
  // req.member is deserialized from the body
  // automatically through the sequelize parse integration
  req.member.save()
    .then(() => {
      res.send(req.member);
    })
    .catch(next);
});

app.updateMember(function (req, res, next) {
  req.member.save()
    .then(() => {
      res.send(req.member);
    })    
    .catch(next);
});

app.getMember(function (req, res, next) {
  res.send(req.member);
});

app.createMemberAddress(function (req, res, next) {
  req.member.addAddress(req.address)
    .then(function (address) {
      res.status(201).send(address);
    })
    .catch(next);
});



```