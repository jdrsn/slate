---
title: API Reference

language_tabs: # must be one of https://git.io/vQNgJ
  - typescript
  - javascript
  - html
  - shell

toc_footers:
  - https://qi.do (base url)
  - <a href='https://c.qi.do' target='_blank'>> console</a>

includes:
  - objects
  - users
  - files
  - extras
  - errors

search: true

code_clipboard: true
---

# Introduction

qi.do is a set of microservices that process the most common backend functions in a software.
That includes real-time database, user authentication, file storage, push notifications, mail delivery,
usage tracking and more.

qi.do relies on a NoSQL, <a href="https://www.mongodb.com/document-databases" target="_blank">document-oriented</a>
database solution which is represented by a set of arrays that store JSON objects.
The database infrastructure runs on distributed instances of <a href="https://www.mongodb.com/" target="_blank">MongoDB</a>.
That means it is possible to migrate any existing MongoDB database to qi.do.

## Concepts

In order to abstract database terminology to programming languages, we like to call databases **apps**, collections **arrays**, and documents **objects**.

The four main concepts on qi.do are:

Term | Description
--------- | ----------- 
object | A document or entry in an array.
array | A collection or table of objects.
app | A database that stores arrays.
microservice | A function that processes apps, arrays or objects.

### Objects

On qi.do, an object follows JSON's structure and its properties (fields/columns) can store values of type:
`boolean`, `numerical`, `string`, `array`, `object` or `null`.

> Example of a JSON object:

```json
{
  "_id": "5fef00ae58daeb0b0b23edcb",
  "name": "Jane Doe",
  "phone": 11901010911,
  "address": [
    {
      "street": "Main Street 43",
      "postalCode": null
    },
    {
      "street": "Jungle Lane 76",
      "postalCode": 99900000
    }
  ],
  "b2b": false
}
```

<br/>
You are able to add, retrieve, change and remove objects.

### Arrays

An array is a collection of JavaScript objects.
Objects can also store another arrays of objects and so on.
If an array does not exist, it is automatically created.

> Example of an array of 3 objects:

```json
[
  {
    "_id": "5fef00ae58daeb0b0b23edcb",
    "name": "Awesome Product",
    "price": 10.5,
    "description": {
      "short": "most awesome product ever",
      "long": "once upon a time..."
    },
    "tags": ["awesome", "service", "product"]
  },
  {
    "_id": "5fef00ae58daeb0b0b23edcb",
    "name": "Nice Product",
    "disabled": true
  },
  {
    "_id": "5fef00ae58daeb0b0b23edcb",
    "name": "Cool Product",
    "price": 8,
    "description": {
      "short": "the coolest product ever"
    }
  }
]
```

### Apps

An app is an instance of qi.do.
In a nutshell, an app is a database with a set of built-in microservices.
Apps need to be created via qi.do console under <a href="https://c.qi.do" target="_blank">c.qi.do</a>.

### Microservices

A microservice executes a specific task always on a given app.
In this API reference, you can find all available microservices that you can use in your apps.

## API architecture

qi.do API is built on <a href="https://en.wikipedia.org/wiki/Representational_state_transfer" target="_blank">REST</a>.
It accepts JSON-encoded and form-encoded request bodies and returns JSON-encoded responses.
Our API has resource-oriented endpoints and uses standard HTTP authentication and response codes.

> Base URL: https://qi.do

The API can be used in any programming language via HTTP.
If you are working with TypeScript or JavaScript, you can use the API through the node package `@qido/client`.

You can use qi.do API directly in your frontend or integrate in your existing backend.

## CRUD microservices

The core of the API is built on <a href="https://en.wikipedia.org/wiki/Create,_read,_update_and_delete" target="_blank">CRUD</a> microservices.

With qi.do, you have your own production-ready API to easily execute the four most common database operations:
`create`, `read`, `update` and `delete` (CRUD).

Also, you can execute all qi.do microservices through authenticated users and even directly from your favorite browser's address bar.

The four main microservices are:

`/c` - `create`

> <a href='https://qi.do/c/app/array?x={"prop":true}' target='_blank'>qi.do/c/app/array?x={"prop":true} </a>

`/r` - `read`

> <a href="https://qi.do/r/app/array" target="_blank">qi.do/r/app/array </a>

`/u` - `update`

> <a href='https://qi.do/u/app/array/id?x={"prop":false}' target='_blank'>qi.do/u/app/array/id?x={"prop":false} </a>

`/d` - `delete`

> <a href="https://qi.do/d/app/array/id" target="_blank">qi.do/d/app/array/id </a>

Almost all HTTP endpoints for CRUD microservices follow the format:

`/<microservice>/<app>/<array>/<data>`

For create `/c` and update `/u` microservices, you can whether pass a JSON object in the HTTP request body, or a JSON
string as the URL query param `x`.

With the read microservice `/r` , you can use URL query params for **retrieving**, **paginating** and **sorting** specific objects.

Our authentication and file storage features are also based on CRUD-like microservices.
See detailed examples in the next sections.

## Advanced microservices

We also offer advanced features such as authentication, real-time database streams, push notifications and more.
All through the same API.

Some advanced endpoints are:

`/a` - `auth`

> <a href='https://qi.do/a/app/user/pass' target='_blank'>qi.do/a/app/user/pass </a>

`/p` - `broadcast`

> <a href='https://qi.do/p/app/b?x={"title":"push","body":"notification"}' target='_blank'>qi.do/p/app/b?x={"title":"push","body":"notification"} </a>

`/m` - `mail`

> <a href='https://qi.do/m/app/5feca140530c0772b232d3e5?x={"to":"mail@example.com","sub":"subject","msg":"test message"}' target='_blank'>qi.do/m/app/5feca140530c0772b232d3e5?x={"to":"mail@example.com","sub":"subject","msg":"test message"} </a>

Most advanced HTTP endpoints follow the format:

`/<microservice>/<app>/<function>/<data>`

# Quickstart

In this section, you will learn how to quickly integrate qi.do to your project and start using it.
You can also try qi.do out by clicking on the links that are shown on top of the code snippets.

### Install, import, init

You can install qi.do's client via npm:

`npm i @qido/client`

```shell
npm i @qido/client
```

The client can be imported in TypeScript or JavaScript code.

```typescript
// import qido client
import * as qido from '@qido/client'
```

```javascript
// import qido client
const qido = require('@qido/client')
```

<br/>
You are free to use our `test` app with no need to register on qi.do.
If you want to use your own app, make sure to replace `test` with your qi.do app name.
An app can be created in just 30s under <a href="https://c.qi.do" target="_blank">c.qi.do</a>. 

```typescript
// init app test
const app = new qido('test')
```

```javascript
// init app test
const app = new qido('test')
```

If your app is secured by API keys, just pass a `key` as second parameter in the initialization.

```typescript
// init app with API key
const app = new qido('test', 'mY4p1k3y')
```

```javascript
// init app with API key
const app = new qido('test', 'mY4p1k3y')
```

### Create user

If your app has user authentication activated, then you have to create and authenticate a user before executing microservices.

A user can be created through the HTTP endpoint `/c/<app>/u` or via `create` method from the node package.

> <a href='https://qi.do/c/test/u/me@example.com/p455w0rd' target='_blank'>qi.do/c/test/u/me@example.com/p455w0rd </a>

```typescript
// user to be created
const user = {
  u: 'me@example.com',
  p: 'p455w0rd',
}
// create a user on the app
app.create('u', user)
  .then(data => console.log(data))
```

```javascript
// get your form element
const form = document.getElementById('user')
// convert to form data
const user = new FormData(form)
// create user
app.create('u', user)
  .then(data => console.log(data))
```

```html
<form id="user">
  <input type="text" name="u" value="me@example.com">
  <input type="text" name="p" value="p455w0rd">
</form>
```

```shell
curl https://qi.do/c/test/u \
-d '{"u":"me@example.com","p":"p455w0rd"}' \
-H 'Content-Type: application/json'
```

### Authenticate user

If your app has authentication on, every request must be authorized through a `token`.
Everytime a user logs in, its `token` is generated.

The method `auth` allows you to authenticate users.
This method corresponds to the HTTP endpoint `/a/<app>`. 

> <a href='https://qi.do/a/test/me@example.com/p455w0rd' target='_blank'>qi.do/a/test/me@example.com/p455w0rd </a>

```typescript
// user data
const login = {
  u: 'me@example.com',
  p: 'p455w0rd'
}
// authenticate user and store token
app.auth(login)
  .then(data => app.token = data.token)
```

```javascript
// get your form element
const form = document.getElementById('login')
// convert to form data
const login = new FormData(form)
// authenticate user and store token
app.auth(login)
  .then(data => app.token = data.token)
```

```html
<form id="login">
  <input type="text" name="u" value="me@example.com">
  <input type="text" name="p" value="p455w0rd">
</form>
```

```shell
curl https://qi.do/a/test/me@example.com/p455w0rd
```

### Create object

The same `create` method for users also allows you to create any kind of JSON object. 
The first parameter takes the name of the array.
The second parameter takes the object to be created.

Alternatively, you can use the HTTP endpoint `/c/<app>/<array>` to create an object.

> <a href='https://qi.do/c/test/prod?x={"name":"pasta","price":12.4,"on":true}' target='_blank'>qi.do/c/test/prod?x={"name":"pasta","price":12.4,"on":true} </a>

```typescript
// object to be created
const product = {
  name: 'pasta',
  price: 12.4,
  on: true
}
// create object and log response
app.create('prod', product)
  .then(res => console.log(res))
```

```javascript
// get your form element
const form = document.getElementById('product')
// convert to form data
const product = new FormData(form)
// create object and log response
app.create('prod', product)
  .then(res => console.log(res))
```

```html
<form id="product">
  <input type="text" name="name" value="pasta">
  <input type="text" name="price" value="12.4">
  <input type="checkbox" name="on" checked>
</form>
```

```shell
curl https://qi.do/c/test/prod \
-d '{"name":"pasta","price":12.4,"on":true}' \
-H 'Content-Type: application/json' \
-H 'Authorization: Bearer token'
```

### Read object

The HTTP endpoint `/r/<app>/<array>` allows you to read objects that are stored in an array.
In the node client library, the `read` method is used for that.

Through the endpoints ending with `<array>`, all objects in this array are retrieved.

> <a href="https://qi.do/r/test/prod" target="_blank">qi.do/r/test/prod </a>

```typescript
// read all objects from prod array
app.read('prod')
  .then(data => console.log(data)) 
```

```javascript
// read all objects from prod array
app.read('prod')
  .then(data => console.log(data))
```

```html

```

```shell
curl https://qi.do/r/test/prod \
-H 'Content-Type: application/json' \
-H 'Authorization: Bearer token'
```

<br/>
For more information about creating, retrieving, updating and deleting objects, please refer to the next sessions.
