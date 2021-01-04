# Objects

On qi.do, an object is the main data structure from which all other structures (like users and files) are inherited.
All kinds of objects on qi.do follow the JSON representation.
An object in a qi.do array can be seen as document or entry in a database table or collection.

## Create an object

The operation `/c` allows you to create an object (document/entry) in an array (collection/table).
A request can be sent through the HTTP methods `POST` and `GET` (if enabled).

### HTTP endpoints

`POST /c/<app>/<array>`

`POST /c/<app>/<array>/<id>`

Using `POST`, the data of the object to be created corresponds to the HTTP request body.

`GET /c/<app>/<array>?x=<JSON>`

`GET /c/<app>/<array>/<id>?x=<JSON>`

Via `GET`, the object to be created needs to be passed as JSON string in the URL query param named `x`.

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

<br/>
The JSON-encoded response contains the `id` of the newly created object.

### URL parameters

In the request URL, you must replace the parameters below with your respective data.

Parameter | Description | Required
--------- | ----------- |  -----------
app | The name of the app to be used. | true
array | The name of the array to be used. | true
id | The id of the object to be created. | false

### Query parameters

If you are using `GET`, the object to be created has to be passed as JSON string in the query param named `x`.

Parameter | Description | Required
--------- | ----------- |  -----------
x | The object to be created. | true







## Read objects

The operation `/r` allows you to read objects (documents/entries) that are stored in a specific array (collection/table).

### Read all objects

`GET /r/<app>/<array>`

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
> HTTP response:

```json
[...] // all objects in the array
```

### Read specific objects

`GET /r/<app>/<array>?x=<JSON>&o=<JSON>`

In order to retrieve specific objects, you have to set a filter to the query param `x` in the request URL.
One can also **sort** and **paginate** queries by sending the options via query param `o`.
For more details about how to query objects with qi.do, please refer to the
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

Both of these query parameters accept JSON string as value.

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

In the request URL, you must replace the parameters below with your respective data.

Parameter | Description | Required
--------- | ----------- |  -----------
app | The name of the app to be used. | true
array | The name of the array to be used. | true
id | The id of the object to be retrieved. | false






## Update an object

The operation `/u` allows you to update an object in an array.
A request can be sent through the HTTP methods `PUT` and `GET` (if enabled).

### HTTP endpoints

`PUT /u/<app>/<array>/<id>`

Using `PUT`, the object data to be updated corresponds to the request body.

`GET /u/<app>/<array>/<id>?x=<JSON>`

Via `GET`, the data needs to be passed as JSON string in the query param `x` of the URL.

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

### URL parameters

In the request URL, you must replace the parameters below with your respective data.

Parameter | Description | Required
--------- | ----------- |  -----------
app | The name of the app to be used. | true
array | The name of the array to be used. | true
id | The id of the object to be updated. | true

### Query parameters

If you are using `GET`, the properties to be updated/added have to be passed in the query param named `x`.

Parameter | Description | Required
--------- | ----------- |  -----------
x | The properties to be updated/added. | true







## Delete an object

The operation `/d` allows you to delete an object from an array.
A request can be sent through the HTTP methods `DELETE` and `GET` (if enabled).

### HTTP endpoints

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

In the request URL, you must replace the parameters below with your respective data.

Parameter | Description | Required
--------- | ----------- |  -----------
app | The name of the app to be used. | true
array | The name of the array to be used. | true
id | The id of the object to be deleted. | true
