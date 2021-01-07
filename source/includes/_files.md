# Files

It is possible to upload files within an object onto any array.

> An arbitrary object before file upload:

```json
{
  "user": "dad",
  "channel": "family",
  "message": "everything is possible",
  "attachments": [], // from input type file
  "path": "custom/path" // the upload path
}
```

<br/>
The uploaded files are referenced in the newly created object.

> The main object after file upload:

```json
{
  "_id": "5feca140530c0772b232d3e5",
  "user": "dad",
  "channel": "family",
  "message": "everything is possible",
  "_f": [ // all uploaded files
    {
      "_id": "5fcbdc6dc5ef0493e50a2fc0",
      "name": "angry-cat.jpg",
      "url": "https://f.qi.do/...",
      "prop": "attachments"
    },
    {
      "_id": "5fcbde89c5ef0493e50a2fc3",
      "name": "farm-bot.pdf",
      "url": "https://f.qi.do/...",
      "prop": "attachments"
    }
  ]
}
```

<br/>
Also, for every file in a request, an object is created in the `f` array of the requested app.

> The file objects created in the `f` array:

```json
[
  {
    "_id": "5fbf9947f54f0cdad0cf1387",
    "path": "custom/path",
    "name": "angry-cat.png",
    "url": "https://f.qi.do...",
    "prop": "attachments",
    "array": "message"
  },
  {
    "_id": "5fbf9947f54f0cdad0cf1388",
    "path": "custom/path",
    "name": "farm-bot.pdf",
    "url": "https://f.qi.do...",
    "prop": "attachments",
    "array": "message"
  }
]
```

## Upload a file (create)

The microservice `/c` also allows you to upload files onto the requested app.
A request can be sent through the HTTP methods `POST` and `PUT` (with microservice `/u`).

### HTTP endpoints

`POST /c/<app>/<array>`

`PUT /u/<app>/<array>`

All files in the same request are uploaded to the `path` specified in the request body.
If a request does not contain `path`, then all files are uploaded to a new automatically created user directory.

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
  <input type="text" name="user" value="dad">
  <input type="text" name="channel" value="family">
  <input type="text" name="text" value="everything is possible">
  <input type="file" name="attachments" multiple>
</form>
```

```shell
curl https://qi.do/c/test/message \
-F 'attachments=@path/to/file.jpg' \
-F 'user="dad"' \
-F 'channel="family"' \
-F 'text="everything is possible"' \
-H 'Authorization: Bearer token'

```

> HTTP response:

```json
{
  "_id": "5fbf9947f54f0cdad0cf1386"
}
```

<br/>
The JSON-encoded response contains the id of the newly created object related to the uploaded files.

### URL parameters

In the request URL, you must replace the parameters below with your respective data.

Parameter | Description | Required
--------- | ----------- |  -----------
app | The name of the app to be used. | true
array | The name of the array to be used. | true
id | The id of the object to be created. | false







## Read files

The microservice `/r/<app>/f` allows you to read files that are stored on an app.

### Read all files

`GET /r/<app>/f`

Through the URLs ending with `/f`, all files on the requested app are retrieved.

> <a href="https://qi.do/r/test/f" target="_blank">qi.do/r/test/f </a>

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
curl https://qi.do/r/test/f \
-H 'Content-Type: application/json' \
-H 'Authorization: Bearer token'
```
> HTTP response:

```json
[...] // all files on the app
```

### Read specific files

`GET /r/<app>/f?x=<JSON>&o=<JSON>`

In order to retrieve specific files, you have to set a filter to the query param `x` in the request URL.
One can also **sort** and **paginate** queries by sending the options via query param `o`.
For more details about how to query files with qi.do, please refer to the
<a href="https://mongodb.github.io/node-mongodb-native/markdown-docs/queries.html#query-object" target="_blank">MongoDB</a>
documentation.

> <a href='https://qi.do/r/test/f?x={"path":"cutom/path"}&o={"limit":2}' target='_blank'>qi.do/r/test/f?x={"path":"cutom/path"}&o={"limit":2} </a>

```typescript
// read specific files on the app
const filter = {path: 'cutom/path'}
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
curl https://qi.do/r/test/f?x={"path":"custom/path"}&o={"limit":2} \
-H 'Content-Type: application/json' \
-H 'Authorization: Bearer token'
```

> HTTP response:

```json
[
  {
    "_id": "5fbf9947f54f0cdad0cf1387",
    "path": "custom/path",
    "name": "angry-cat.png",
    "url": "https://f.qi.do...",
    "prop": "attachments",
    "array": "message",
    "_uid": "5fbf99d47f54df0dad0f1323"
  },
  {
    "_id": "5fbf9947f54f0cdad0cf1388",
    "path": "custom/path",
    "name": "farm-bot.pdf",
    "url": "https://f.qi.do...",
    "prop": "attachments",
    "array": "message",
    "_uid": "5fbf99d47f54df0dad0f1323"
  }
]
```

### Query parameters

Both of these query parameters accept JSON string as value.

Parameter | Description | Required
--------- | ----------- |  -----------
x | The query filter object. | false
o | The query options object. | false

### Read a single file

`GET /r/<app>/f/<id>`

A single user can be retrieved by attaching its id to the request URL.

> <a href="https://qi.do/r/test/f/5fcbde89c5ef0493e50a2fc3" target="_blank">qi.do/r/test/f/5fcbde89c5ef0493e50a2fc3 </a>

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
curl https://qi.do/r/test/f/5fbf9947f54f0cdad0cf1387 \
-H 'Content-Type: application/json' \
-H 'Authorization: Bearer token'
```

> HTTP response:

```json
{
  "_id": "5fbf9947f54f0cdad0cf1387",
  "path": "custom/path",
  "name": "angry-cat.png",
  "url": "https://f.qi.do...",
  "prop": "attachments",
  "array": "message",
  "_uid": "5fbf99d47f54df0dad0f1323"
}
```

### URL parameters

In the request URL, you must replace the parameters below (except `f`) with your respective data.

Parameter | Description | Required
--------- | ----------- |  -----------
app | The name of the app to be used. | true
f | The default file array. | true
id | The id of the file to be retrieved. | false








## Delete a file

The microservice `/d/<app>/f` allows you to delete a file from an app.
A request can be sent through the HTTP methods `DELETE` and `GET` (if enabled).

### HTTP endpoints

`DELETE /d/<app>/f/<id>`

`GET /d/<app>/f/<id>`

Using both methods, it is just necessary to append the file id in the URL.

> <a href="https://qi.do/d/test/f/5fcbdeb1c5ef0493e50a2fc4" target="_blank">qi.do/d/test/f/5fcbdeb1c5ef0493e50a2fc4 </a>

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
curl https://qi.do/d/test/f/5fcbdeb1c5ef0493e50a2fc4 \
--request DELETE \
-H 'Authorization: Bearer token'
```

> HTTP response:

```json
200
```

### URL parameters

In the request URL, you must replace the parameters below (except `f`) with your respective data.

Parameter | Description | Required
--------- | ----------- |  -----------
app | The name of the app to be used. | true
f | The default file array. | true
id | The id of the file to be deleted. | true
