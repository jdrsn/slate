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

