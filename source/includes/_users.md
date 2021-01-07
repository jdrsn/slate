# Users

A user object looks almost like any other object.
The main difference is that the properties `u` (username or email) and `p` (password) are required for a user to log in.

> Example of a user object:

```json
{
  "_id": "5feca140530c0772b232d3e5",
  "u": "me@example.com",
  "p": "p455w0rd",
  "mood": "lockdown suicidal",
  "disturb": true
}
```

<br/>
You are free to add any data to a `u` object.

## Create a user

The microservice `/c/<app>/u` allows you to create a user in the requested app.
A request can be sent through the HTTP methods `POST` and `GET` (if enabled).

### HTTP endpoints

`POST /c/<app>/u`

Using `POST`, the data of the user to be created corresponds to the request body.

`GET /c/<app>/u/<user>/<pass>`

Via `GET`, username (or e-mail) and password can be passed directly as URL params.

`GET /c/<app>/u?x=<JSON>`

For `GET` requests, the user data can also be passed as JSON string in the query param `x` of the URL.

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

> HTTP Response:

```json
{
  "_id": "5feca140530c0772b232d3e5"
}
```

<br/>
The JSON-encoded response contains the id of the newly created user.
A custom user id can be set directly in the request body as `_id`.
The `p` property is encrypted and never shown in the results.

### URL parameters

In the request URL, you must replace the parameters below (except `u`) with your respective data.

Parameter | Description | Required
--------- | ----------- |  -----------
app | The name of the app to be used. | true
u | The default user array. | true
user | The username or email of the user. | false
pass | The password of the user to be created. | false

### Query parameters

If you are using `GET`, the object to be created has to be passed as JSON string in the query param `x`.

Parameter | Description | Required
--------- | ----------- |  -----------
x | The user to be created. | true






## Authenticate a user

The microservice `/a` allows a user to authenticate with email (or username) and password. 
A request can be sent through the HTTP methods `POST` and `GET` (if enabled).

### HTTP endpoints

`POST /a/<app>`

Using `POST`, user/email and password of the user to be authenticated corresponds to the request body.

`GET /a/<app>/<user>/<pass>`

`GET /a/<app>/<user>/<pass>/<expires>`

Via `GET`, the login data should be in the URL parameters.

> <a href='https://qi.do/a/test/me@example.com/p455w0rd' target='_blank'>qi.do/a/test/me@example.com/p455w0rd </a>

```typescript
// user data
const login = {
  u: 'me@example.com',
  p: 'p455w0rd',
  e: '7d'
}
// authenticate user
app.auth(login)
  .then(data => console.log(data))
```

```javascript
// get your form element
const form = document.getElementById('login')
// convert to form data
const login = new FormData(form)
// authenticate user
app.auth(login)
  .then(data => console.log(data))
```

```html
<form id="login">
  <input type="text" name="u" value="me@example.com">
  <input type="text" name="p" value="p455w0rd">
  <input type="text" name="e" value="7d">
</form>
```

```shell
curl https://qi.do/a/test/me@example.com/p455w0rd/7d
```

> HTTP Response:

```json
{
  "token": "eyJhbGciOiJIUzI1NiIs..."
}
```

<br/>
If the user data matches, a JWT (JSON Web Token) is sent with the response.
All the other microservices can then be authorized with this token.

### URL parameters

In the request URL, you must replace the parameters below with your respective data.

Parameter | Description | Required
--------- | ----------- |  -----------
app | The name of the app to be used. | true
user | The user (email address) to log in. | false
pass | The password of the user. | false
expires | The time period until which the token should be valid, in the form `30s`, `2h`, `7d`. Default value is `1h`. | false







## Read users

The microservice `/r/<app>/u` allows you to read users that are registered on an app.

### Read all users

`GET /r/<app>/u`

Through the URLs ending with `/u`, all users on the requested app are retrieved.

> <a href="https://qi.do/r/test/u" target="_blank">qi.do/r/test/u </a>

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
curl https://qi.do/r/test/u \
-H 'Content-Type: application/json' \
-H 'Authorization: Bearer token'
```
> HTTP response:

```json
[...] // all users on the app
```

### Read specific users

`GET /r/<app>/u?x=<JSON>&o=<JSON>`

In order to retrieve specific users, you have to set a filter to the query param `x` in the request URL.
One can also **sort** and **paginate** queries by sending the options via query param `o`.
For more details about how to query users with qi.do, please refer to the
<a href="https://mongodb.github.io/node-mongodb-native/markdown-docs/queries.html#query-object" target="_blank">MongoDB</a>
documentation.

> <a href='https://qi.do/r/test/u?x={"disabled":true}&o={"limit":3}' target='_blank'>qi.do/r/test/u?x={"disabled":true}&o={"limit":3} </a>

```typescript
// read specific users
const filter = {disabled: true}
const options = {limit: 3}
app.read('u', filter, options)
  .then(users => console.log(users))
```

```javascript
// read specific users
const filter = {disabled: true}
const options = {limit: 3}
app.read('u', filter, options)
  .then(users => console.log(users))
```

```html
```

```shell
curl https://qi.do/r/test/u?x={"disabled":true}&o={"limit":3} \
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
    "u": "joe",
    "disabled": true
  },
  {
    "_id": "5fcbde89c2ef0433e50a2fc7",
    "u": "pam",
    "disabled": true
  }
]
```

### Query parameters

Both of these query parameters accept JSON string as value.

Parameter | Description | Required
--------- | ----------- |  -----------
x | The query filter object. | false
o | The query options object. | false

### Read a single user

`GET /r/<app>/u/<id>`

A single user can be retrieved by attaching its id to the request URL.

> <a href="https://qi.do/r/test/u/5fcbde89c5ef0493e50a2fc3" target="_blank">qi.do/r/test/u/5fcbde89c5ef0493e50a2fc3 </a>

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
curl https://qi.do/r/test/u/5fcbde89c5ef0493e50a2fc3 \
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

In the request URL, you must replace the parameters below (except `u`) with your respective data.

Parameter | Description | Required
--------- | ----------- |  -----------
app | The name of the app to be used. | true
u | The default user array. | true
id | The id of the user to be retrieved. | false







## Update a user

The microservice `/u/<app>/u` allows you to update a user on an app.
A request can be sent through the HTTP methods `PUT` and `GET` (if enabled).

### HTTP endpoints

`PUT /u/<app>/u/<id>`

Using `PUT`, the data of the user to be updated corresponds to the request body.

`GET /u/<app>/u/<id>?x=<JSON>`

Via `GET`, the data needs to be passed as JSON string in the query param `x` of the URL.

> <a href='https://qi.do/u/test/u?x={"mood":"endless boredom","disturb":true}' target='_blank'>qi.do/u/test/u?x={"mood":"endless boredom","disturb":true} </a>

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
  <input type="text" name="mood" value="endless boredom">
  <input type="checkbox" name="disturb" checked>
</form>
```

```shell
curl https://qi.do/u/test/u/5fcbdc57c5ef0493e50a2fbd \
--request PUT \
--data '{"mood":"endless boredom","disturb":true}' \
-H 'Content-Type: application/json' \
-H 'Authorization: Bearer token'
```

> HTTP response:

```json
200
```

### URL parameters

In the request URL, you must replace the parameters below (except `u`) with your respective data.

Parameter | Description | Required
--------- | ----------- |  -----------
app | The name of the app to be used. | true
u | The default user array. | true
id | The id of the user to be updated. | true

### Query parameters

If you are using `GET`, the properties to be updated/added have to be passed in the query param `x`.

Parameter | Description | Required
--------- | ----------- |  -----------
x | The properties to updated/added. | true







## Delete a user

The microservice `/d/<app>/u` allows you to delete a user from an app.
A request can be sent through the HTTP methods `DELETE` and `GET` (if enabled).

### HTTP Request

`DELETE /d/<app>/u/<id>`

`GET /d/<app>/u/<id>`

Using both methods, it is just necessary to append the user id in the URL.

> <a href="https://qi.do/d/test/u/5fcbdeb1c5ef0493e50a2fc4" target="_blank">qi.do/d/test/u/5fcbdeb1c5ef0493e50a2fc4 </a>

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
curl https://qi.do/d/test/u/5fcbdeb1c5ef0493e50a2fc4 \
--request DELETE \
-H 'Authorization: Bearer token'
```

> HTTP response:

```json
200
```

### URL parameters

In the request URL, you must replace the parameters below (except `u`) with your respective data.

Parameter | Description | Required
--------- | ----------- |  -----------
app | The name of the app to be used. | true
u | The default user array. | true
id | The id of the user to be deleted. | true
