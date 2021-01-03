# Objects

With `qi.do`, you have your own production-ready API to easily execute the four most common database operations (CRUD),
also through authenticated users, even directly from your favorite browser's address bar.

These operations are:

`C` for `create`: `qi.do/c`
`R` for `read`: `qi.do/r`
`U` for `update`: `qi.do/u`
`D` for `delete`: `qi.do/d`

All URLs follow the format `qi.do/<operation>/<app>/<array>/<objectId>`.
For create `/c` and update `/u` operations, you can whether pass a JSON object in the HTTP request body or a JSON
string as a query param named "x" in the URL.
With read `/r` operations, you can use URL query params for retrieving specific objects.
`qi.do`'s authentication and storage services are also based on CRUD-like operations.
See detailed examples in the next sections.

## Create an object

The operation `/c` allows you to create an object (document/entry) in an array (collection/table).
A request can be sent through the HTTP methods `POST` and `GET` (if enabled).

### HTTP request

`POST /c/<app>/<array>`

Using `POST`, the data of the object to be created corresponds to the HTTP request body.

`GET /c/<app>/<array>?x=JSON_STRING`

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

> HTTP response:

```json
{
  "_id": "5feca140530c0772b232d3e5"
}
```

You can also define an `id` for the object that you are creating by attaching it to the request URL, like `.../<array>/<id>`.

### URL parameters

In the request URL, you must replace these parameters with your respective data.

Parameter | Description | Required
--------- | ----------- |  -----------
app | The name of the app to be used. | true
array | The name of the array to be used. | true
id | The id of the object to be created. | false

### Query parameters (`GET` only)

Parameter | Description | Required
--------- | ----------- |  -----------
x | The object to create as JSON string. | true







## Read objects

The operation `/r` allows you to read objects (documents/entries) that are stored in a specific array (collection/table).
A request can be sent only via HTTP method `GET`.

### Read all objects

`GET /r/<app>/<array>`

Through the URLs ending with `/<array>`, all objects in this array are retrieved.

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
[...] // all objects in array
```

### Read specific objects

`GET /r/<app>/<array>?x=JSON_STRING&o=JSON_STRING`

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

`GET /r/<app>/<array>/<id>`

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

In the request URL, you must replace `app`, `array` and `id` with your respective data.

Parameter | Description | Required
--------- | ----------- |  -----------
app | The name of the app to be used. | true
array | The name of the array to be used. | true
id | The id of the object to be retrieved. | false






## Update an object

The operation `/u` allows you to update an object in an array.
A request can be sent through the HTTP methods `PUT` and `GET` (if enabled).

### HTTP Request

`PUT /u/<app>/<array>/<id>`

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
  <input type="text" name="mood" value="endless boredom">
  <input type="checkbox" name="disturb" checked>
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

`GET /u/<app>/<array>/<id>?x=JSON_STRING`

Via `GET`, the data needs to be passed as a JSON string in the query param `x` of the URL.

### URL parameters

In the request URL, you must replace `app`, `array` and `id` with your respective data.

Parameter | Description | Required
--------- | ----------- |  -----------
app | The name of the app to be used. | true
array | The name of the array to be used. | true
id | The id of the object to be updated. | true

### Query parameters (`GET` only)

If you are using `GET`, the properties to be updated/added have to be passed in the query param named `x`.

Parameter | Description | Required
--------- | ----------- |  -----------
x | The properties to update/add as JSON string. | true







## Delete an object

The operation `/d` allows you to delete an object from an array.
A request can be sent through the HTTP methods `DELETE` and `GET` (if enabled).

### HTTP Request


`DELETE /d/<app>/<array>/<id>`

`GET /d/<app>/<array>/<id>`

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

In the request URL, you must replace `app`, `array` and `id` with your respective data.

Parameter | Description | Required
--------- | ----------- |  -----------
app | The name of the app to be used. | true
array | The name of the array to be used. | true
id | The id of the object to be deleted. | true
