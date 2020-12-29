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

Welcome to the Kittn API! You can use our API to access Kittn API endpoints, which can get information on various cats, kittens, and breeds in our database.

We have language bindings in Shell, JavaScript and TypeScript! You can view code examples in the dark area to the right, and you can switch the programming language of the examples with the tabs in the top right.

This example API documentation page was created with [Slate](https://github.com/slatedocs/slate). Feel free to edit it and use it as a base for your own API's documentation.

# Quick-Start

The API can be used in any programming language via HTTP. However, there is a client library for NodeJS.  

## Installation

You can install qido's client via npm:

`npm i @qido/client`

## Import

The client can be imported on JavaScript or TypeScript code.

```javascript
const client = require('@qido/client')
```

```typescript
import * as client from '@qido/client'
```

## Initialization

Make sure to replace `app` with your app name.

```javascript
const qido = new client('app')
```

```typescript
const qido = new client('app')
```

# Authentication

If your app has authentication on, every request must be authorized through a token. Everytime a user logs in, a `token` is generated. A user can be logged in via HTTP under `https://qi.do/a` or through the libraries.

```javascript
qido.auth('u@u.uu', 'uuuuuu').then(data => console.log(data))
```

```typescript
qido.auth('u@u.uu', 'uuuuuu').then(data => console.log(data))
```

```shell
curl "https://qi.do/a/app/u@u.uu/uuuuuu"
```

## HTTP Request

`GET https://qi.do/a/app/user/pass`

`GET https://qi.do/a/app/user/pass/7d`

```json
{
  "success": "user authenticated",
  "data": {
    "u": "u@u.uu",
    "uid": "5feb740358daeb0b0b23edba",
    "token": "eyJhbGciOiJIUzI1NiIs..."
  }
}
```

## URL Parameters

You must replace `app`, `user` and `pass` with your respective data directly in the request URL.

Parameter | Required | Description
--------- | ----------- |  -----------
app | true | The name of the app to be used.
user | true | The user (email address) to log in.
pass | true | The password of the user.
expires | false | The time that the token should be valid in the form `30s`, `2h`, `7d`. Default value is `1h`.

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

