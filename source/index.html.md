---
title: API Reference

language_tabs: # must be one of https://git.io/vQNgJ
  - javascript
  - typescript
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
const app = new client('app')
```

```typescript
const app = new client('app')
```

Make sure to replace `app` with your app name.

## Authentication

```javascript
// authenticate user on qi.do
app.auth('u@u.uu', 'uuuuuu')
  .then(data => console.log(data))
```

```typescript
app.auth('u@u.uu', 'uuuuuu')
  .then(data => console.log(data))
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

`GET https://qi.do/a/app/user/pass`

`GET https://qi.do/a/app/user/pass/7d`

### URL Parameters

In the request URL, you must replace `app`, `user` and `pass` with your respective data.

Parameter | Description | Required
--------- | ----------- |  -----------
app | The name of the app to be used. | true
user | The user (email address) to log in. | true
pass | The password of the user. | true
expires | The time period until which the token should be valid, in the form `30s`, `2h`, `7d`. Default value is `1h`. | false









# Objects

## Create An Object

```javascript
// JSON object to be created
const object = {
  random: 123,
  stuff: [
    {
      cool: true,
      text: "a test"
    },
    {
      foo: null
    }
  ]
}
// create object on qi.do and log response
app.create('array', object, token)
  .then(data => console.log(data))
```

```typescript
// JSON object to be created
const object = {
  random: 123,
  stuff: [
    {
      cool: true,
      text: "a test"
    },
    {
      foo: null
    }
  ]
}
// create object on qi.do and log response
app.create('array', object, token)
  .then(data => console.log(data))
```

```shell
curl https://qi.do/c/app/array \
-d '{"random":123,"stuff":[{"foo":true,"text":"a test"},{"foo":null}]}' \
-H 'Content-Type: application/json' \
-H 'Authorization: Bearer token'
```

> HTTP Response:

```json
{
  "success": "object created",
  "data": {
    "_id": "5feca140530c0772b232d3e5",
  }
}
```

The operation `/c` allows you to create an object (document/entry) inside an array (collection/table).
A request can be sent through the HTTP methods `POST` and `GET` (if enabled).
Using `POST`, the data of the object to be created corresponds to the request body.
Via `GET`, the data needs to be passed as a JSON-string in the query param `x` of the URL, such like `qi.do/c/app/array?x=json_string`.

### HTTP Request

`POST https://qi.do/c/app/array`

`GET https://qi.do/c/app/array?x={"random":123,"stuff":[{"foo":true,"text":"a test"},{"foo":null}]}`

### URL Parameters

In the request URL, you must replace `app` and `array` with your respective data.

Parameter | Description | Required
--------- | ----------- |  -----------
app | The name of the app to be used. | true
array | The name of the array to be used. | true

### Query Parameters (`GET` Only)

If you are using `GET`, the JSON object to be created has to be passed in the query param named `x`.

Parameter | Description | Required
--------- | ----------- |  -----------
x | The object to create as JSON string. | true

## Read All Objects

```javascript
// read all objects from the array
app.read('array', token)
  .then(data => console.log(data))
```

```typescript
// read all objects from the array
app.read('array', token)
  .then(data => console.log(data))
```

```shell
curl https://qi.do/r/app/array \
-H 'Content-Type: application/json' \
-H 'Authorization: Bearer token'
```

> HTTP Response:

```json
[
  {
    "_id": "8e0EWCO9nCWOtF54YmKF",
    "uid": "8n3IXnCkEfP6...",
    "random": 123,
    "stuff": [
      {
        "foo": true,
        "text": "a test :)"
      },
      {
        "foo": null
      }
    ],
    "_p": 0
  },
  {
    "_id": "YROrVTebxH8CRgd6MOhq",
    "uid": "J8mrNRkcqaP1...",
    "random": 123,
    "stuff": null
  },
  {
    "_id": "aCustomGivenId",
    "uid": "J8mrNRkcqaP1...",
    "random": 456,
    "otherStuff": [
      true,
      false
    ]
  }
]
```

The operation `/r` allows you to read objects (documents/entries) that are stored in a specific array (collection/table).
A request can be sent only via HTTP method `GET`.
Through the URLs ending with `/array`, all objects inside this array are retrieved.

### HTTP Request

`GET https://qi.do/r/app/array`

### URL Parameters

In the request URL, you must replace `app` and `array` with your respective data.

Parameter | Description | Required
--------- | ----------- |  -----------
app | The name of the app to be used. | true
array | The name of the array to be used. | true

## Read Specific Objects

```javascript
// read object with id objectId
app.read('array/objectId', token)
  .then(data => console.log(data))
```

```typescript
// read object with id objectId
app.read('array/objectId', token)
  .then(data => console.log(data))
```

```shell
curl https://qi.do/r/app/array/objectId \
-H 'Content-Type: application/json' \
-H 'Authorization: Bearer token'
```

> HTTP Response:

```json
{
  "_id": "objectId",
  "uid": "J8mrNRkcqaP1...",
  "random": 123,
  "stuff": null
}
```

A single object can be retrieved by attaching its `id` to the request URL in the form `qi.do/r/app/array/objectId`.
It is also possible to execute filter queries to return specific objects within an array.

### HTTP Request

`GET https://qi.do/r/app/array/objectId`

### URL Parameters

In the request URL, you must replace `app` and `array` with your respective data.

Parameter | Description | Required
--------- | ----------- |  -----------
app | The name of the app to be used. | true
array | The name of the array to be used. | true









## Update An Object

```javascript
// JSON properties to be updated/added
const props = {
  random: 456,
  otherStuff: [true, false]
}
// update object by its id
app.update('array/objectId', props, token)
  .then(data => console.log(data))
```

```typescript
// JSON properties to be updated/added
const props = {
  random: 456,
  otherStuff: [true, false]
}
// update object by its id
app.update('array/objectId', props, token)
  .then(data => console.log(data))
```

```shell
curl https://qi.do/u/app/array/objecId \
--request PUT \
--data '{"random":456,"otherStuff":[true,false]}' \
-H 'Content-Type: application/json' \
-H 'Authorization: Bearer token'
```

> HTTP Response:

```json
{
  "success": "object updated"
}
```

The operation `/u` allows you to update an object inside an array.
A request can be sent through the HTTP methods `PUT` and `GET` (if enabled).
Using `PUT`, the data of the object to be updated corresponds to the request body.
Via `GET`, the data needs to be passed as a JSON-string in the query param `x` of the URL, such like `qi.do/u/app/array/objectId?x=json_string`.

### HTTP Request

`PUT https://qi.do/u/app/array/objectId`

`GET https://qi.do/u/app/array/objectId?x={"random":456,"otherStuff":[true,false]}`

### URL Parameters

In the request URL, you must replace `app` and `array` with your respective data.

Parameter | Description | Required
--------- | ----------- |  -----------
app | The name of the app to be used. | true
array | The name of the array to be used. | true

### Query Parameters (`GET` Only)

If you are using `GET`, the properties to be updated/added have to be passed in the query param named `x`.

Parameter | Description | Required
--------- | ----------- |  -----------
x | The properties to update/add as JSON string. | true










## Delete An Object

```javascript
// delete object by its id
app.delete('array/objectId', token)
  .then(data => console.log(data))
```

```typescript
// delete object by its id
app.delete('array/objectId', token)
  .then(data => console.log(data))
```

```shell
curl https://qi.do/d/app/array/objecId \
--request DELETE \
-H 'Authorization: Bearer token'
```

> HTTP Response:

```json
{
  "success": "object deleted"
}
```

The operation `/d` allows you to delete an object from an array.
A request can be sent through the HTTP methods `DELETE` and `GET` (if enabled).
Using both methods, it is just necessary to append the object id in the URL, such as `qi.do/d/app/array/objectId`.

### HTTP Request

`DELETE https://qi.do/d/app/array/objectId`

`GET https://qi.do/d/app/array/objectId`






# Users

A user object is just like any other object.
Only the properties `u` (email) and `p` (password) are required.
A custom user id can be set directly in the request body as `_id`.









## Create A User

```javascript
// the user to be created
const user = {
  u: "test@qi.do",
  p: "p455w0rd",
}
// create a user on qi.do
app.create('u', user)
  .then(data => console.log(data))
```

```typescript
// the user to be created
const user = {
  u: "test@qi.do",
  p: "p455w0rd",
}
// create a user on qi.do
app.create('u', user)
  .then(data => console.log(data))
```

```shell
curl https://qi.do/c/app/u \
-d '{"u":"test@qi.do","p""p455w0rd"}' \
-H 'Content-Type: application/json'
```

> HTTP Response:

```json
{
  "success": "object created",
  "data": {
    "_id": "5feca140530c0772b232d3e5",
  }
}
```

The operation `/c/app/u` allows you to create a user in the requested app.
A request can be sent through the HTTP methods `POST` and `GET` (if enabled).
Using `POST`, the data of the user to be created corresponds to the request body.
Via `GET`, the data can be passed as a JSON string in the query param `x` of the URL, such like `qi.do/c/app/u?x=json_string`.
E-mail and password can be passed directly as URL params.

### HTTP Request

`POST https://qi.do/c/app/u`

`GET https://qi.do/c/app/u/test@qi.do/p455w0rd`

### URL Parameters

In the request URL, you must replace `app` with your respective data.

Parameter | Description | Required
--------- | ----------- |  -----------
app | The name of the app to be used. | true
u | The user array. | true

### Query Parameters (`GET` Only)

If you are using `GET`, the JSON object to be created has to be passed in the query param named `x`.

Parameter | Description | Required
--------- | ----------- |  -----------
x | The user to create as JSON string. | true






## Read A User

```javascript
// read user with id userId
app.read('u/userId', token)
  .then(data => console.log(data))
```

```typescript
// read user with id userId
app.read('u/userId', token)
  .then(data => console.log(data))
```

```shell
curl https://qi.do/r/app/u/objectId \
-H 'Content-Type: application/json' \
-H 'Authorization: Bearer token'
```

> HTTP Response:

```json
{
  "_id": "userId",
  "u": "test@qi.do",
  "displayName": "John Doe",
  "disabled": false
}
```

A single user can be retrieved by attaching its `id` to the request URL in the form `qi.do/r/app/u/userId`.
It is also possible to execute filter queries to return specific users.

### HTTP Request

`GET https://qi.do/r/app/u/userId`

### URL Parameters

In the request URL, you must replace `app` with your respective data.

Parameter | Description | Required
--------- | ----------- |  -----------
app | The name of the app to be used. | true
u | The user array. | true







## Update A User

```javascript
// JSON properties to be updated/added
const props = {
  disabled: false,
  p: "n3wP455w0rd"
}
// update user via token
app.update('u', props, token)
  .then(data => console.log(data))
```

```typescript
// JSON properties to be updated/added
const props = {
  disabled: false,
  p: "n3wP455w0rd"
}
// update user via token
app.update('u', props, token)
  .then(data => console.log(data))
```

```shell
curl https://qi.do/u/app/u \
--request PUT \
--data '{"disabled":false,"p":"n3wP455w0rd"}' \
-H 'Content-Type: application/json' \
-H 'Authorization: Bearer token'
```

> HTTP Response:

```json
{
  "success": "object updated"
}
```

The operation `/u/app/u` allows a user to update its own data.
A request can be sent through the HTTP methods `PUT` and `GET` (if enabled).
Using `PUT`, the data of the user to be updated corresponds to the request body.
Via `GET`, the data needs to be passed as a JSON-string in the query param `x` of the URL, such like `qi.do/u/app/u?x=json_string`.

### HTTP Request

`PUT https://qi.do/u/app/u`

`GET https://qi.do/u/app/u?x={"random":456,"otherStuff":[true,false]}`

### URL Parameters

In the request URL, you must replace `app` with your respective data.

Parameter | Description | Required
--------- | ----------- |  -----------
app | The name of the app to be used. | true
u | The user array. | true

### Query Parameters (`GET` Only)

If you are using `GET`, the properties to be updated/added have to be passed in the query param named `x`.

Parameter | Description | Required
--------- | ----------- |  -----------
x | The properties to update/add as JSON string. | true






## Delete A User

```javascript
// delete user via token
app.delete('u/u@u.uu/uuuuuu', token)
  .then(data => console.log(data))
```

```typescript
// delete user via token
app.delete('u/u@u.uu/uuuuuu', token)
  .then(data => console.log(data))
```

```shell
curl https://qi.do/d/app/u \
--request DELETE \
--data '{"u":"u@u.uu","p":"uuuuuu"}' \
-H 'Content-Type: application/json' \
-H 'Authorization: Bearer token'
```

> HTTP Response:

```json
{
  "success": "object deleted"
}
```
The operation `/d/app/u` allows a user to delete its own user account.
A request can be sent through the HTTP methods `DELETE` and `GET` (if enabled).
Using both methods, it is just necessary to append email and password in the URL, such as `qi.do/d/app/u/email/password`.

A user can only be deleted if email and password are sent with the request.
Only the user from the current authorization token can be updated/deleted.

### HTTP Request

`DELETE https://qi.do/d/app/u`

`GET https://qi.do/d/app/u/test@qi.do/p455w0rd`





# Files

A file object is also just like any other object.
For every file in the request, an object in the `f` array is created.
The data of the uploaded files are also stored in the newly created object of the requested array.
That means it is possible to upload files onto no mather what array.











## Create A File

```javascript
// the object with files to be uploaded
const object = {
  otherStuff: "everything is possible",
  files: [],
  path: "custom/path"
}
// create a file on qi.do
app.create('array', object, token)
  .then(data => console.log(data))
```

```typescript
// the object with files to be uploaded
const object = {
  otherStuff: "everything is possible",
  files: [],
  path: "custom/path"
}
// create a file on qi.do
app.create('array', object, token)
  .then(data => console.log(data))
```

```shell
curl https://qi.do/c/app/array \
-d '{"files":[],"path":"custom/path"}' \
-H 'Content-Type: application/json'
```

> HTTP Response:

```json
{
  "success": "object created",
  "data": {
    "_id": "5fbf9947f54f0cdad0cf1386",
    "otherStuff": "everything is possible",
    "files": [
      {
        "_id": "5fbf9947f54f0cdad0cf1387",
        "field": "fotos",
        "array": "array",
        "path": "custom/path",
        "name": "fileName.png",
        "url": "https://f.qi.do...",
        "uid": "5fbe9efd8370f4c15f6e91d6"
      },
      {
        "_id": "5fbf9947f54f0cdad0cf1388",
        "field": "fotos",
        "array": "array",
        "path": "custom/path/file.pdf",
        "name": "file.pdf",
        "url": "https://f.qi.do...",
        "uid": "5fbe9efd8370f4c15f6e91d6"
      }
    ],
    "uid": "5fbe9efd8370f4c15f6e91d6"
  }
}
```

The operation `/c` allows you to upload files onto the requested app.
A request can be sent through the HTTP methods `POST` and `PUT` (with operation `/u`).
All files in the same request are upload to the specified `path` in the request body.
If a request does not contain `path`, then all files are uploaded to a new automatically created user directory.

### HTTP Request

`POST https://qi.do/c/app/array`

`PUT https://qi.do/u/app/array`

### URL Parameters

In the request URL, you must replace `app` with your respective data.

Parameter | Description | Required
--------- | ----------- |  -----------
app | The name of the app to be used. | true
array | The name of the array to be used. | true






## Read Files

```javascript
// read file with id fileId
app.read('f/fileId', token)
  .then(data => console.log(data))
```

```typescript
// read file with id fileId
app.read('f/fileId', token)
  .then(data => console.log(data))
```

```shell
curl https://qi.do/r/app/f/fileId \
-H 'Content-Type: application/json' \
-H 'Authorization: Bearer token'
```

> HTTP Response:

```json
{
  "_id": "5fbf9947f54f0cdad0cf1388",
  "field": "files",
  "array": "array",
  "path": "custom/path/file.pdf",
  "name": "file.pdf",
  "url": "https://f.qi.do...",
  "uid": "5fbe9efd8370f4c15f6e91d6"
}
```

The operation `/r/app/f` allows a user to read its own files.
A request can be sent only via HTTP `GET` method.
A single file can be retrieved by sending its ID in the request URL, such like `qi.do/r/app/f/fileId`.
Through the URLs ending with `/f`, one can read all files or filter them as any other query.

### HTTP Request

`GET https://qi.do/r/app/f/fileId`

`GET https://qi.do/r/app/f`

`GET https://qi.do/r/app/f?x={"path":"path/to/files"}`

### URL Parameters

In the request URL, you must replace `app` with your respective data.

Parameter | Description | Required
--------- | ----------- |  -----------
app | The name of the app to be used. | true
f | The file array. | true






## Delete A File

```javascript
// delete file by its id
app.delete('f/fileId', token)
  .then(data => console.log(data))
```

```typescript
// delete file by its id
app.delete('f/fileId', token)
  .then(data => console.log(data))
```

```shell
curl https://qi.do/d/app/f/fileId \
--request DELETE \
-H 'Authorization: Bearer token'
```

> HTTP Response:

```json
{
  "success": "object deleted"
}
```
The operation `/d` allows a user to delete its own files.
A request can be sent through the HTTP methods `DELETE` and `GET` (if enabled).


### HTTP Request

`DELETE https://qi.do/d/app/f/fileId`

`GET https://qi.do/d/app/f/fileId`








# Kittens

## Get All Kittens


```typescript
import kittn

api = kittn.authorize('meowmeowmeow')
api.kittens.get()
```

```shell
curl "http://example.com/api/kittens" \
  -H "Authorization: meowmeowmeow"
```

```javascript
const kittn = require('kittn');

let api = kittn.authorize('meowmeowmeow');
let kittens = api.kittens.get();
```

> The above command returns JSON structured like this:

```json
[
  {
    "id": 1,
    "name": "Fluffums",
    "breed": "calico",
    "fluffiness": 6,
    "cuteness": 7
  },
  {
    "id": 2,
    "name": "Max",
    "breed": "unknown",
    "fluffiness": 5,
    "cuteness": 10
  }
]
```

This endpoint retrieves all kittens.

### HTTP Request

`GET http://example.com/api/kittens`

### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
include_cats | false | If set to true, the result will also include cats.
available | true | If set to false, the result will include kittens that have already been adopted.

<aside class="success">
Remember — a happy kitten is an authenticated kitten!
</aside>

## Get a Specific Kitten

```typescript
import kittn

api = kittn.authorize('meowmeowmeow')
api.kittens.get(2)
```

```shell
curl "http://example.com/api/kittens/2" \
  -H "Authorization: meowmeowmeow"
```

```javascript
const kittn = require('kittn');

let api = kittn.authorize('meowmeowmeow');
let max = api.kittens.get(2);
```

> The above command returns JSON structured like this:

```json
{
  "id": 2,
  "name": "Max",
  "breed": "unknown",
  "fluffiness": 5,
  "cuteness": 10
}
```

This endpoint retrieves a specific kitten.

<aside class="warning">Inside HTML code blocks like this one, you can't use Markdown, so use <code>&lt;code&gt;</code> blocks to denote code.</aside>

### HTTP Request

`GET http://example.com/kittens/<ID>`

### URL Parameters

Parameter | Description
--------- | -----------
ID | The ID of the kitten to retrieve

## Delete a Specific Kitten

```typescript
import kittn

api = kittn.authorize('meowmeowmeow')
api.kittens.delete(2)
```

```shell
curl "http://example.com/api/kittens/2" \
  -X DELETE \
  -H "Authorization: meowmeowmeow"
```

```javascript
const kittn = require('kittn');

let api = kittn.authorize('meowmeowmeow');
let max = api.kittens.delete(2);
```

> The above command returns JSON structured like this:

```json
{
  "id": 2,
  "deleted" : ":("
}
```

This endpoint deletes a specific kitten.

### HTTP Request

`DELETE http://example.com/kittens/<ID>`

### URL Parameters

Parameter | Description
--------- | -----------
ID | The ID of the kitten to delete

