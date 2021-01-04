# Files

A file object also just looks like any other object.
For every file in the request, an object in the `f` array is created.
The data of the uploaded files are also stored in the newly created object of the requested array.
That means it is possible to upload files onto no mather which array.

## Upload a file (create)

The operation `/c` also allows you to upload files onto the requested app.
A request can be sent through the HTTP methods `POST` and `PUT` (with operation `/u`).

### HTTP endpoints

`POST /c/<app>/<array>`

`PUT /u/<app>/<array>`

All files in the same request are uploaded to the `_path` specified in the request body.
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
-F 'attachments=@path/to/file.jpg' \
-F 'user="dad"' \
-F 'channel="family"' \
-F 'text="everything is possible"' \
-H 'Authorization: Bearer token'

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

In the request URL, you must replace the parameters below with your respective data.

Parameter | Description | Required
--------- | ----------- |  -----------
app | The name of the app to be used. | true
array | The name of the array to be used. | true







## Read files

The operation `/r/<app>/f` allows you to read files that are stored on an app.

### Read all files

`GET /r/<app>/f`

Through the URLs ending with `/f`, all files on the requested app are retrieved.

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

`GET /r/<app>/f?x=<JSON>&o=<JSON>`

In order to retrieve specific files, you have to set a filter to the query param `x` in the request URL.
One can also **sort** and **paginate** queries by sending the options via query param `o`.
For more details about how to query files with qi.do, please refer to the
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
    "name": "farm-bot.pdf",
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

`GET /r/<app>/f/<id>`

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

In the request URL, you must replace the parameters below with your respective data.

Parameter | Description | Required
--------- | ----------- |  -----------
app | The name of the app to be used. | true
u | The default user array. | true
id | The id of the file to be retrieved. | false








## Delete a file

The operation `/d/<app>/u` allows you to delete a file from an app.
A request can be sent through the HTTP methods `DELETE` and `GET` (if enabled).

### HTTP endpoints

`DELETE /d/<app>/u/<id>`

`GET /d/<app>/u/<id>`

Using both methods, it is just necessary to append the file `id` in the URL.

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

In the request URL, you must replace the parameters below with your respective data.

Parameter | Description | Required
--------- | ----------- |  -----------
app | The name of the app to be used. | true
f | The default file array. | true
id | The id of the file to be deleted. | true
