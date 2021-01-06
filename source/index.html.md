---
title: API Reference

language_tabs: # must be one of https://git.io/vQNgJ
  - typescript
  - javascript
  - html
  - shell

toc_footers:
  - <a href='#'>Sign Up for a Developer Key</a>
  - <a href='https://github.com/slatedocs/slate'>Documentation Powered by Slate</a>

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

qi.do offers a NoSQL, <a href="https://www.mongodb.com/document-databases" target="_blank">document-oriented</a>
database solution which is represented by a set of arrays that store JSON objects.
The database infrastructure runs on distributed instances of <a href="https://www.mongodb.com/" target="_blank">MongoDB</a>.

For many reasons, qi.do uses a simpler terminology. On qi.do, databases are called **apps**, collections are named **arrays**, and documents **objects**.

**Objects**

On qi.do, an object (document/entry) follows JSON's structure and its properties (fields/columns) can contain values with type:
`boolean`, `numerical`, `string`, `array`, `object` or `null`. You are able to add, retrieve, change and remove objects.

**Arrays**

An array is a collection of JavaScript objects that can contain another arrays of objects and so on.
If an array does not exist, it is automatically created.

> Example of an array of 3 objects:

```json
[
  {
    "_id": "YROrVTebxH8CRgd6MOhq",
    "random": 10.5,
    "_p": 0,
    "_uid": "J8mrNRkcqaP1..."
  },
  {
    "_id": "8e0EWCO9nCWOtF54YmKF",
    "random": 123,
    "stuff": [
      {
        "foo": true,
        "text": "a test :)"
      },
      {
        "foo": null
      }
    ]
  },
  {
    "_id": "aCustomGivenId",
    "random": 456
  }
]
```

**Apps**

On qi.do, an app is a set of functions that rely on a database.
Every array with their objects always belongs to an app.
It takes only 30s to create an app via qi.do's console under <a href="https://c.qi.do" target="_blank">c.qi.do</a>.

### API reference

qi.do API is built on <a href="https://en.wikipedia.org/wiki/Representational_state_transfer" target="_blank">REST</a>.
It accepts JSON-encoded and form-encoded request bodies and returns JSON-encoded responses.
Our API has resource-oriented endpoints and uses standard HTTP authentication and response codes.

> Base URL: https://qi.do

The API can be used in any programming language via HTTP.
If you are working with TypeScript or JavaScript, you can use the node package `@qido/client`.

### CRUD operations

The core of the API is built on CRUD operations.

With qi.do, you have your own production-ready API to easily execute the four most common database operations:
`create`, `read`, `update` and `delete` (CRUD).

Also, you can execute these operations through authenticated users and even directly from your favorite browser's address bar.

The four main operations are:

> <a href='https://qi.do/c/app/array?x={"prop":true}' target='_blank'>qi.do/c/app/array?x={"prop":true} </a>

`/c` for `create`

> <a href="https://qi.do/r/app/array" target="_blank">qi.do/r/app/array </a>

`/r` for `read`

> <a href='https://qi.do/u/app/array/id?x={"prop":false}' target='_blank'>qi.do/u/app/array/id?x={"prop":false} </a>

`/u` for `update`

> <a href="https://qi.do/d/app/array/id" target="_blank">qi.do/d/app/array/id </a>

`/d` for `delete`

The advanced endpoints are:

> <a href='https://qi.do/a/app/user/pass' target='_blank'>qi.do/a/app/user/pass </a>

`/a` for `auth`

> <a href='https://qi.do/x/app/p/b?x={"title":"push","body":"notification"}' target='_blank'>qi.do/x/app/p/b?x={"title":"push","body":"notification"} </a>

`/x` for microservice operations

Almost all HTTP endpoints follow the format:

`/<operation>/<app>/<array>/<data>`

For create `/c` and update `/u` operations, you can whether pass a JSON object in the HTTP request body, or a JSON
string as a query param named `x` in the URL.

With read `/r` operations, you can use URL query params for retrieving, paginating and sorting specific objects.
Our authentication and storage services are also based on CRUD-like operations.
See detailed examples in the next sections.

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
import * as qido from '@qido/client'
```

```javascript
const qido = require('@qido/client')
```

<br/>
You are free to use our `test` app without the need to register on qi.do.
If you want to use your own app, make sure to replace `test` with your desired app name.
An app can be created in leen than 1 minute under <a href="https://c.qi.do" target="_blank">c.qi.do</a>. 

```typescript
const app = new qido('test')
```

```javascript
const app = new qido('test')
```

### Create user

If your app has authentication activated, then you have to create and authenticate a user before executing operations.

A user can be created through the HTTP endpoint `/c/<app>/u` or via `create` method.

> <a href='https://qi.do/c/chat/u/dad@example.com/p455w0rd' target='_blank'>qi.do/c/chat/u/dad@example.com/p455w0rd </a>

```typescript
// user to be created
const user = {
  u: 'dad@example.com',
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
  <input type="text" name="u" value="dad@example.com">
  <input type="text" name="p" value="p455w0rd">
</form>
```

```shell
curl https://qi.do/c/chat/u \
-d '{"u":"dad@example.com","p":"p455w0rd"}' \
-H 'Content-Type: application/json'
```

### Authenticate user

If your app has authentication on, every request must be authorized through a `token`. Everytime a user logs in, its `token` is generated.

The method `auth` allows you to authenticate users. This method corresponds to the HTTP endpoint `/a/<app>`. 

> <a href='https://qi.do/a/chat/dad@example.com/p455w0rd' target='_blank'>qi.do/a/chat/dad@example.com/p455w0rd </a>

```typescript
// user data
const login = {
  u: 'dad@example.com',
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
  <input type="text" name="u" value="dad@example.com">
  <input type="text" name="p" value="p455w0rd">
</form>
```

```shell
curl https://qi.do/a/chat/dad@example.com/p455w0rd
```

### Create object

The same `create` method for users also allows you to create any kind of JSON object. 
The first parameter takes the name of the array.
The second parameter takes the object to be created.

Alternatively, you can use the HTTP endpoint `/c/<app>/<array>` to create an object.

> <a href='https://qi.do/c/chat/message?x={"user":"dad","channel":"kids","text":"dinner is ready :)"}' target='_blank'>qi.do/c/chat/message?x={"user":"dad","channel":"kids","text":"dinner is ready :)"} </a>

```typescript
// object to be created
const message = {
  user: 'dad',
  channel: 'kids',
  text: 'dinner is ready :)'
}
// create object and log response
app.create('message', message)
  .then(data => console.log(data))
```

```javascript
// get your form element
const form = document.getElementById('message')
// convert to form data
const message = new FormData(form)
// create message along with all files
app.create('message', message)
  .then(data => console.log(data))
```

```html
<form id="message">
  <input type="text" name="user" value="dad">
  <input type="text" name="channel" value="kids">
  <input type="text" name="text" value="dinner is ready :)">
</form>
```

```shell
curl https://qi.do/c/chat/message \
-d '{"user":"dad","channel":"kids","text":"dinner is ready :)"}' \
-H 'Content-Type: application/json' \
-H 'Authorization: Bearer token'
```

### Read object

The HTTP endpoint `/r/<app>/<array>` allows you to read objects that are stored in an array.
In the node client library, the `read` method is used for that.

Through the endpoints ending with `<array>`, all objects in this array are retrieved.

> <a href="https://qi.do/r/chat/message" target="_blank">qi.do/r/chat/message </a>

```typescript
// read all objects from message array
app.read('message')
  .then(data => console.log(data)) 
```

```javascript
// read all objects from message array
app.read('message')
  .then(data => console.log(data))
```

```html

```

```shell
curl https://qi.do/r/chat/message \
-H 'Content-Type: application/json' \
-H 'Authorization: Bearer token'
```

<br/>
For more information about creating, retrieving, updating and deleting objects, please refer to the next sessions.
