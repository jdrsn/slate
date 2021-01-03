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
  - errors

search: true

code_clipboard: true
---

# Introduction

qi.do offers a NoSQL database solution which is represented by a set of arrays (collections/tables) that store JSON objects (documents/entries).
An object follows JSON's structure and its properties (fields/columns) can contain values with type boolean, numerical, string, array, object or null.
An array corresponds to a collection of JavaScript objects that can contain another arrays of objects and so on.
On qi.do, an array is automatically created if it does not exist.

# Quick-Start

The API can be used in any programming language via HTTP. However, there is a client library on `npm`.  

## Installation

```shell
npm i @qido/client
```

You can install qi.do's client via npm:

`npm i @qido/client`

## Import

```javascript
const client = require('@qido/client')
```

```typescript
import * as client from '@qido/client'
```

The client can be imported on JavaScript or TypeScript code.

## Initialization

```javascript
const app = new client('chat')
```

```typescript
const app = new client('chat')
```

Make sure to replace `app` with your app name.

## Authentication

```typescript
app.auth('u@u.uu', 'uuuuuu')
  .then(data => console.log(data))
```

```javascript
// authenticate user on qi.do
app.auth('u@u.uu', 'uuuuuu')
  .then(data => console.log(data))
```

```html
<form id="message">
  <input type="text" name="channel" placeholder="channel">
  <input type="text" name="text" placeholder="message">
  <input type="file" name="attachments" multiple>
</form>
```

```shell
curl https://qi.do/a/app/u@u.uu/uuuuuu
```

> HTTP Response:

```json
{
  "success": "user authenticated",
  "data": {
    "uid": "5feb740358daeb0b0b23edba",
    "token": "eyJhbGciOiJIUzI1NiIs..."
  }
}
```

If your app has authentication on, every request must be authorized through a `token`. Everytime a user logs in, a `token` is generated. A user can be logged in via HTTP under `https://qi.do/a` or through the libraries.

### HTTP Request

`GET /a/app/profile/pass`

`GET /a/app/profile/pass/7d`

### URL parameters

In the request URL, you must replace `app`, `user` and `pass` with your respective data.

Parameter | Description | Required
--------- | ----------- |  -----------
app | The name of the app to be used. | true
user | The user (email address) to log in. | true
pass | The password of the user. | true
expires | The time period until which the token should be valid, in the form `30s`, `2h`, `7d`. Default value is `1h`. | false









# Objects

## Create an object

The operation `/c` allows you to create an object (document/entry) inside an array (collection/table).
A request can be sent through the HTTP methods `POST` and `GET` (if enabled).

### HTTP request

`POST /c/app/array`

Using `POST`, the data of the object to be created corresponds to the HTTP request body.

`GET /c/app/array?x=JSON_STRING`

If you are using `GET`, the object to be created needs to be passed as JSON string in the URL query param named `x`.

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
  <input type="text" name="user">
  <input type="text" name="channel">
  <input type="text" name="text">
</form>
```

```shell
curl https://qi.do/c/chat/message \
-d '{"user":"dad","channel":"kids","text":"dinner is ready :)"}' \
-H 'Content-Type: application/json' \
-H 'Authorization: Bearer token'
```

> HTTP response:

```json
{
  "_id": "5feca140530c0772b232d3e5"
}
```

You can also define an `ID` for the object that you are creating by attaching it to the request URL, like `.../array/objectId`. 

### URL parameters

In the request URL, you must replace these parameters with your respective data.

Parameter | Description | Required
--------- | ----------- |  -----------
app | The name of the app to be used. | true
array | The name of the array to be used. | true
objectId | The id of the object to be created. | false

### Query parameters (`GET` only)

Parameter | Description | Required
--------- | ----------- |  -----------
x | The object to create as JSON string. | true







## Read objects

The operation `/r` allows you to read objects (documents/entries) that are stored in a specific array (collection/table).
A request can be sent only via HTTP method `GET`.

### Read all objects

`GET /r/app/array`

Through the URLs ending with `/array`, all objects inside this array are retrieved.

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
> HTTP response:

```json
[...] // all objects inside array
```

### Read specific objects

`GET /r/app/array?x=JSON_STRING&o=JSON_STRING`

In order to retrieve specific objects, you have to set a filter to the query param `x` in the request URL.
One can also **sort** and **paginate** queries by sending the options via query param `o`.
For more details about how to query objects with `qi.do`, please refer to the
<a href="https://mongodb.github.io/node-mongodb-native/markdown-docs/queries.html#query-object" target="_blank">MongoDB</a>
documentation.

> <a href='https://qi.do/r/chat/message?x={"channel":"kids"}&o={"limit":3}' target='_blank'>qi.do/r/chat/message?x={"channel":"kids"}&o={"limit":3} </a>

```typescript
// read specific objects from message array
const filter = {channel: 'kids'}
const options = {limit: 3}
app.read('message', filter, options)
  .then(data => console.log(data))
```

```javascript
// read specific objects from message array
const filter = {channel: 'kids'}
const options = {limit: 3}
app.read('message', filter, options)
  .then(data => console.log(data))
```

```html

```

```shell
curl https://qi.do/r/chat/message?x={"channel":"kids"}&o={"limit":3} \
-H 'Content-Type: application/json' \
-H 'Authorization: Bearer token'
```

> HTTP response:

```json
[
  {
    "_id": "5feca140530c0772b232d3e5",
    "user": "dad",
    "channel": "kids",
    "text": "dinner is ready :)"
  },
  {
    "_id": "5fcbde89c5ef0493e50a2fc3",
    "user": "jay",
    "channel": "kids",
    "text": "morning <0/"
  },
  {
    "_id": "5fcbdc6dc5ef0493e50a2fc0",
    "user": "mia",
    "channel": "kids",
    "text": "i'm coming"
  }
]
```

### Query parameters

Both of these query parameters accept a JSON string as value.

Parameter | Description | Required
--------- | ----------- |  -----------
x | The query filter object. | false
o | The query options object. | false

### Read a single object

`GET /r/app/array/objectId`

A single object can be retrieved by attaching its `id` to the request URL.

> <a href="https://qi.do/r/chat/message/5fcbde89c5ef0493e50a2fc3" target="_blank">qi.do/r/chat/message/5fcbde89c5ef0493e50a2fc3 </a>

```typescript
// read object by id
app.read('message/5fcbde89c5ef0493e50a2fc3')
  .then(data => console.log(data))
```

```javascript
// read object by id
app.read('message/5fcbde89c5ef0493e50a2fc3')
  .then(data => console.log(data))
```

```html

```

```shell
curl https://qi.do/r/chat/message/5fcbde89c5ef0493e50a2fc3 \
-H 'Content-Type: application/json' \
-H 'Authorization: Bearer token'
```

> HTTP response:

```json
{
  "_id": "5fcbde89c5ef0493e50a2fc3",
  "user": "jay",
  "channel": "kids",
  "text": "morning <0/"
}
```

### URL parameters

In the request URL, you must replace `app` and `array` with your respective data.

Parameter | Description | Required
--------- | ----------- |  -----------
app | The name of the app to be used. | true
array | The name of the array to be used. | true
objectId | The id of the object to be retrieved. | false






## Update an object

The operation `/u` allows you to update an object inside an array.
A request can be sent through the HTTP methods `PUT` and `GET` (if enabled).

### HTTP Request

`PUT /u/app/array/objectId`

Using `PUT`, the data of the object to be updated corresponds to the request body.

> <a href='https://qi.do/u/chat/user?x={"mood":"endless boredom","disturb":true}' target='_blank'>qi.do/u/chat/user?x={"mood":"endless boredom","disturb":true} </a>

```typescript
// properties to be updated/added
const profile = {
  mood: 'endless boredom',
  disturb: true
}
// update object by id
app.update('profile/5fcbdc57c5ef0493e50a2fbd', profile)
  .then(data => console.log(data))
```

```javascript
// get your form element
const form = document.getElementById('profile')
// convert to form data
const profile = new FormData(form)
// update profile
app.update('profile/5fcbdc57c5ef0493e50a2fbd', profile)
  .then(res => console.log(res))
```

```html
<form id="profile">
  <input type="text" name="mood">
  <input type="checkbox" name="disturb">
</form>
```

```shell
curl https://qi.do/u/chat/profile/5fcbdc57c5ef0493e50a2fbd \
--request PUT \
--data '{"mood":"endless boredom","disturb":true}' \
-H 'Content-Type: application/json' \
-H 'Authorization: Bearer token'
```

> HTTP response:

```json
200
```

`GET /u/app/array/objectId?x=JSON_STRING`

Via `GET`, the data needs to be passed as a JSON string in the query param `x` of the URL.

### URL parameters

In the request URL, you must replace `app`, `array` and `objectId` with your respective data.

Parameter | Description | Required
--------- | ----------- |  -----------
app | The name of the app to be used. | true
array | The name of the array to be used. | true
objectId | The id of the object to be updated. | true

### Query parameters (`GET` only)

If you are using `GET`, the properties to be updated/added have to be passed in the query param named `x`.

Parameter | Description | Required
--------- | ----------- |  -----------
x | The properties to update/add as JSON string. | true







## Delete an object

The operation `/d` allows you to delete an object from an array.
A request can be sent through the HTTP methods `DELETE` and `GET` (if enabled).

### HTTP Request


`DELETE /d/app/array/objectId`

`GET /d/app/array/objectId`

Using both methods, it is just necessary to append the object id in the URL.

> <a href="https://qi.do/d/chat/message/5fcbdeb1c5ef0493e50a2fc4" target="_blank">qi.do/d/chat/message/5fcbdeb1c5ef0493e50a2fc4 </a>

```typescript
// delete object by id
app.delete('message/5fcbdeb1c5ef0493e50a2fc4')
  .then(data => console.log(data))
```

```javascript
// delete object by id
app.delete('message/5fcbdeb1c5ef0493e50a2fc4')
  .then(data => console.log(data))
```

```html

```

```shell
curl https://qi.do/d/chat/message/5fcbdeb1c5ef0493e50a2fc4 \
--request DELETE \
-H 'Authorization: Bearer token'
```

> HTTP response:

```json
200
```

### URL parameters

In the request URL, you must replace `app`, `array` and `objectId` with your respective data.

Parameter | Description | Required
--------- | ----------- |  -----------
app | The name of the app to be used. | true
array | The name of the array to be used. | true
objectId | The id of the object to be deleted. | true








# Users

A user object behaves almost like any other object.
The main difference is that the properties `u` (username or email) and `p` (password) are required.
A custom user id can be set directly in the request body as `_id`.





## Create a user

The operation `/c/app/u` allows you to create a user in the requested app.
A request can be sent through the HTTP methods `POST` and `GET` (if enabled).

### HTTP request

`POST /c/app/u`

Using `POST`, the data of the user to be created corresponds to the request body.

`GET /c/app/u/user/pass`

Via `GET`, username (or e-mail) and password can be passed directly as URL params.

`GET /c/app/u?x=JSON_STRING`

For `GET` requests, the user data can also be passed as a JSON string in the query param `x` of the URL. 

> <a href='https://qi.do/c/chat/u/test@qi.do/p455w0rd' target='_blank'>qi.do/c/chat/u/test@qi.do/p455w0rd </a>

```typescript
// user to be created
const user = {
  u: 'test@qi.do',
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
  <input type="text" name="u">
  <input type="text" name="p">
</form>
```

```shell
curl https://qi.do/c/app/u \
-d '{"u":"test@qi.do","p""p455w0rd"}' \
-H 'Content-Type: application/json'
```

> HTTP Response:

```json
{
  "_id": "5feca140530c0772b232d3e5"
}
```

<br/>
You are free to add any data to the `u` object. The `p` property is encrypted and never shown in the results.

### URL parameters

In the request URL, you must replace `app`, `user` and `pass` with your respective data.

Parameter | Description | Required
--------- | ----------- |  -----------
app | The name of the app to be used. | true
u | The default user array. | true
user | The username or email of the user. | false
pass | The password of the user to be created. | false

### Query parameters (`GET` only)

If you are using `GET`, the object to be created has to be passed as JSON string in the query param named `x`.

Parameter | Description | Required
--------- | ----------- |  -----------
x | The user to be created. | true







## Read users

The operation `/r/app/u` allows you to read users that are registered on the `app`.
A request can be sent only via HTTP method `GET`.

### Read all users

`GET /r/app/u`

Through the URLs ending with `/u`, all users on the `app` are retrieved.

> <a href="https://qi.do/r/chat/u" target="_blank">qi.do/r/chat/u </a>

```typescript
// read all users on the app
app.read('u')
  .then(data => console.log(data))
```

```javascript
// read all users on the app
app.read('u')
  .then(data => console.log(data))
```

```html

```

```shell
curl https://qi.do/r/chat/u \
-H 'Content-Type: application/json' \
-H 'Authorization: Bearer token'
```
> HTTP response:

```json
[...] // all users on the app
```

### Read specific users

`GET /r/app/u?x=JSON_STRING&o=JSON_STRING`

In order to retrieve specific users, you have to set a filter to the query param `x` in the request URL.
One can also **sort** and **paginate** queries by sending the options via query param `o`.
For more details about how to query users with `qi.do`, please refer to the
<a href="https://mongodb.github.io/node-mongodb-native/markdown-docs/queries.html#query-object" target="_blank">MongoDB</a>
documentation.

> <a href='https://qi.do/r/chat/u?x={"disabled":true}&o={"limit":3}' target='_blank'>qi.do/r/chat/u?x={"disabled":true}&o={"limit":3} </a>

```typescript
// read specific objects from message array
const filter = {channel: 'kids'}
const options = {limit: 3}
app.read('u', filter, options)
  .then(users => console.log(users))
```

```javascript
// read specific objects from message array
const filter = {disabled: true}
const options = {limit: 3}
app.read('u', filter, options)
  .then(users => console.log(users))
```

```html

```

```shell
curl https://qi.do/r/chat/u?x={"disabled":true}&o={"limit":3} \
-H 'Content-Type: application/json' \
-H 'Authorization: Bearer token'
```

> HTTP response:

```json
[
  {
    "_id": "5feca140530c0772b232d3e5",
    "u": "kai",
    "disabled": true
  },
  {
    "_id": "5fcbde89c5ef0493e50a2fc3",
    "u": "tai",
    "disabled": true
  },
  {
    "_id": "5fcbde89c2ef0433e50a2fc7",
    "u": "nai",
    "disabled": true
  }
]
```

### Query parameters

Both of these query parameters accept a JSON string as value.

Parameter | Description | Required
--------- | ----------- |  -----------
x | The query filter object. | false
o | The query options object. | false

### Read a single user

`GET /r/app/u/userId`

A single user can be retrieved by attaching its `id` to the request URL.

> <a href="https://qi.do/r/chat/u/5fcbde89c5ef0493e50a2fc3" target="_blank">qi.do/r/chat/u/5fcbde89c5ef0493e50a2fc3 </a>

```typescript
// read user by id
app.read('u/5fcbde89c5ef0493e50a2fc3')
  .then(user => console.log(user))
```

```javascript
// read user by id
app.read('u/5fcbde89c5ef0493e50a2fc3')
  .then(user => console.log(user))
```

```html

```

```shell
curl https://qi.do/r/chat/u/5fcbde89c5ef0493e50a2fc3 \
-H 'Content-Type: application/json' \
-H 'Authorization: Bearer token'
```

> HTTP response:

```json
{
  "_id": "5fcbde89c5ef0493e50a2fc3",
  "u": "jay",
  "disabled": false
}
```

### URL parameters

In the request URL, you must replace `app` and `userId` with your respective data.

Parameter | Description | Required
--------- | ----------- |  -----------
app | The name of the app to be used. | true
u | The default user array. | true
userId | The id of the user to be retrieved. | false







## Update a user

The operation `/u/app/u` allows you to update a `user` on an `app`.
A request can be sent through the HTTP methods `PUT` and `GET` (if enabled).

### HTTP Request

`PUT /u/app/u/userId`

Using `PUT`, the data of the user to be updated corresponds to the request body.

> <a href='https://qi.do/u/chat/u?x={"mood":"endless boredom","disturb":true}' target='_blank'>qi.do/u/chat/u?x={"mood":"endless boredom","disturb":true} </a>

```typescript
// properties to be updated/added
const user = {
  mood: 'endless boredom',
  disturb: true
}
// update user by id
app.update('u/5fcbdc57c5ef0493e50a2fbd', user)
  .then(res => console.log(res))
```

```javascript
// get your form element
const form = document.getElementById('user')
// convert to form data
const user = new FormData(form)
// create message along with all files
app.update('u/5fcbdc57c5ef0493e50a2fbd', user)
  .then(res => console.log(res))
```

```html
<form id="user">
  <input type="text" name="mood">
  <input type="checkbox" name="disturb">
</form>
```

```shell
curl https://qi.do/u/chat/u/5fcbdc57c5ef0493e50a2fbd \
--request PUT \
--data '{"mood":"endless boredom","disturb":true}' \
-H 'Content-Type: application/json' \
-H 'Authorization: Bearer token'
```

> HTTP response:

```json
200
```

`GET /u/app/u/userId?x=JSON_STRING`

Via `GET`, the data needs to be passed as a JSON string in the query param `x` of the URL.

### URL parameters

In the request URL, you must replace `app` and `userId` with your respective data.

Parameter | Description | Required
--------- | ----------- |  -----------
app | The name of the app to be used. | true
u | The default user array. | true
userId | The id of the user to be updated. | true

### Query parameters (`GET` only)

If you are using `GET`, the properties to be updated/added have to be passed in the query param named `x`.

Parameter | Description | Required
--------- | ----------- |  -----------
x | The properties to update/add as JSON string. | true







## Delete a user

The operation `/d/app/u` allows you to delete a user from an `app`.
A request can be sent through the HTTP methods `DELETE` and `GET` (if enabled).

### HTTP Request

`DELETE /d/app/u/userId`

`GET /d/app/u/userId`

Using both methods, it is just necessary to append the user id in the URL.

> <a href="https://qi.do/d/chat/u/5fcbdeb1c5ef0493e50a2fc4" target="_blank">qi.do/d/chat/u/5fcbdeb1c5ef0493e50a2fc4 </a>

```typescript
// delete user by id
app.delete('u/5fcbdeb1c5ef0493e50a2fc4')
  .then(res => console.log(res))
```

```javascript
// delete user by id
app.delete('u/5fcbdeb1c5ef0493e50a2fc4')
  .then(res => console.log(res))
```

```html

```

```shell
curl https://qi.do/d/chat/u/5fcbdeb1c5ef0493e50a2fc4 \
--request DELETE \
-H 'Authorization: Bearer token'
```

> HTTP response:

```json
200
```

### URL parameters

In the request URL, you must replace `app` and `userId` with your respective data.

Parameter | Description | Required
--------- | ----------- |  -----------
app | The name of the app to be used. | true
u | The default user array. | true
userId | The id of the user to be deleted. | true








# Files

A file object also just looks like any other object.
For every file in the request, an object in the `f` array is created.
The data of the uploaded files are also stored in the newly created object of the requested array.
That means it is possible to upload files onto no mather which array.






## Upload a file (create)

The operation `/c` also allows you to upload files onto the requested app.
A request can be sent through the HTTP methods `POST` and `PUT` (with operation `/u`).

### HTTP Request

`POST /c/app/array`

`PUT /u/app/array`

All files in the same request are uploaded to the specified `_path` in the request body.
If a request does not contain `_path`, then all files are uploaded to a new automatically created user directory.

```typescript
// get your form element
const form = document.getElementById('message')
// convert to form data
const message = new FormData(form)
// create message along with all files
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
  <input type="text" name="user">
  <input type="text" name="channel">
  <input type="text" name="text">
  <input type="file" name="attachments" multiple>
</form>
```

```shell
curl https://qi.do/c/chat/message \
-d '{"files":[],"path":"custom/path"}' \
-H 'Content-Type: application/json'
```

> HTTP response:

```json
{
  "_id": "5fbf9947f54f0cdad0cf1386",
  "user": "dad",
  "channel": "family",
  "text": "everything is possible",
  "_f": [
    {
      "_id": "5fbf9947f54f0cdad0cf1387",
      "name": "angry-cat.png",
      "url": "https://f.qi.do...",
      "prop": "attachments"
    },
    {
      "_id": "5fbf9947f54f0cdad0cf1388",
      "name": "farm-robot.pdf",
      "url": "https://f.qi.do...",
      "prop": "attachments"
    }
  ]
}
```

### URL parameters

In the request URL, you must replace `app` and `array` with your respective data.

Parameter | Description | Required
--------- | ----------- |  -----------
app | The name of the app to be used. | true
array | The name of the array to be used. | true







## Read files

The operation `/r/app/f` allows you to read files that are stored on the `app`.
A request can be sent only via HTTP method `GET`.

### Read all files

`GET /r/app/f`

Through the URLs ending with `/f`, all files on the `app` are retrieved.

> <a href="https://qi.do/r/chat/f" target="_blank">qi.do/r/chat/f </a>

```typescript
// read all files on the app
app.read('f')
  .then(data => console.log(data))
```

```javascript
// read all files on the app
app.read('f')
  .then(data => console.log(data))
```

```html

```

```shell
curl https://qi.do/r/chat/f \
-H 'Content-Type: application/json' \
-H 'Authorization: Bearer token'
```
> HTTP response:

```json
[...] // all files on the app
```

### Read specific files

`GET /r/app/f?x=JSON_STRING&o=JSON_STRING`

In order to retrieve specific files, you have to set a filter to the query param `x` in the request URL.
One can also **sort** and **paginate** queries by sending the options via query param `o`.
For more details about how to query files with `qi.do`, please refer to the
<a href="https://mongodb.github.io/node-mongodb-native/markdown-docs/queries.html#query-object" target="_blank">MongoDB</a>
documentation.

> <a href='https://qi.do/r/chat/f?x={"_path":"cutom/path"}&o={"limit":2}' target='_blank'>qi.do/r/chat/f?x={"_path":"cutom/path"}&o={"limit":2} </a>

```typescript
// read specific files on the app
const filter = {_path: 'cutom/path'}
const options = {limit: 2}
app.read('f', filter, options)
  .then(files => console.log(files))
```

```javascript
// read specific files on the app
const filter = {path: 'custom/path'}
const options = {limit: 2}
app.read('f', filter, options)
  .then(files => console.log(files))
```

```html

```

```shell
curl https://qi.do/r/chat/f?x={"_path":"custom/path"}&o={"limit":2} \
-H 'Content-Type: application/json' \
-H 'Authorization: Bearer token'
```

> HTTP response:

```json
[
  {
    "_id": "5fbf9947f54f0cdad0cf1387",
    "_path": "custom/path",
    "name": "angry-cat.png",
    "url": "https://f.qi.do...",
    "prop": "attachments",
    "array": "message",
    "_uid": "5fbf99d47f54df0dad0f1323"
  },
  {
    "_id": "5fbf9947f54f0cdad0cf1388",
    "_path": "custom/path",
    "name": "farm-robot.pdf",
    "url": "https://f.qi.do...",
    "prop": "attachments",
    "array": "message",
    "_uid": "5fbf99d47f54df0dad0f1323"
  }
]
```

### Query parameters

Both of these query parameters accept a JSON string as value.

Parameter | Description | Required
--------- | ----------- |  -----------
x | The query filter object. | false
o | The query options object. | false

### Read a single file

`GET /r/app/f/fileId`

A single user can be retrieved by attaching its `id` to the request URL.

> <a href="https://qi.do/r/chat/f/5fcbde89c5ef0493e50a2fc3" target="_blank">qi.do/r/chat/f/5fcbde89c5ef0493e50a2fc3 </a>

```typescript
// read file by id
app.read('f/5fcbde89c5ef0493e50a2fc3')
  .then(user => console.log(user))
```

```javascript
// read file by id
app.read('f/5fcbde89c5ef0493e50a2fc3')
  .then(user => console.log(user))
```

```html

```

```shell
curl https://qi.do/r/chat/f/5fbf9947f54f0cdad0cf1387 \
-H 'Content-Type: application/json' \
-H 'Authorization: Bearer token'
```

> HTTP response:

```json
{
  "_id": "5fbf9947f54f0cdad0cf1387",
  "_path": "custom/path",
  "name": "angry-cat.png",
  "url": "https://f.qi.do...",
  "prop": "attachments",
  "array": "message",
  "_uid": "5fbf99d47f54df0dad0f1323"
}
```

### URL parameters

In the request URL, you must replace `app` and `fileId` with your respective data.

Parameter | Description | Required
--------- | ----------- |  -----------
app | The name of the app to be used. | true
u | The default user array. | true
fileId | The id of the file to be retrieved. | false








## Delete a file

The operation `/d/app/u` allows you to delete a file from an `app`.
A request can be sent through the HTTP methods `DELETE` and `GET` (if enabled).

### HTTP Request

`DELETE /d/app/u/fileId`

`GET /d/app/u/fileId`

Using both methods, it is just necessary to append the file id in the URL.

> <a href="https://qi.do/d/chat/f/5fcbdeb1c5ef0493e50a2fc4" target="_blank">qi.do/d/chat/f/5fcbdeb1c5ef0493e50a2fc4 </a>

```typescript
// delete file by id
app.delete('f/5fcbdeb1c5ef0493e50a2fc4')
  .then(res => console.log(res))
```

```javascript
// delete file by id
app.delete('f/5fcbdeb1c5ef0493e50a2fc4')
  .then(res => console.log(res))
```

```html

```

```shell
curl https://qi.do/d/chat/f/5fcbdeb1c5ef0493e50a2fc4 \
--request DELETE \
-H 'Authorization: Bearer token'
```

> HTTP response:

```json
200
```

### URL parameters

In the request URL, you must replace `app` and `fileId` with your respective data.

Parameter | Description | Required
--------- | ----------- |  -----------
app | The name of the app to be used. | true
f | The default file array. | true
fileId | The id of the user to be deleted. | true


