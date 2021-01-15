# Objects

On qi.do, an object is the main data structure from which all other structures (such as users and files) are inherited.
All kinds of objects on qi.do follow the JSON representation.
An object in a qi.do array can be seen as document or entry in a database table or collection.

> An arbitrary object:

```json
{
  "_id": "5feca140530c0772b232d3e5",
  "items": [3, 5, 8],
  "total": 47.3,
  "comment": "deliver at door"
}
```

<br/>
Any object can have any properties you wish. You are free to implement your own data interfaces even directly from your frontend application.

## Create an object

The microservice `/c` allows you to create an object (document/entry) in an array (collection/table).
A request can be sent through the HTTP methods `POST` and `GET` (if enabled).

### HTTP endpoints

`POST /c/:app/:array`

`POST /c/:app/:array/:id`

Using `POST`, the data of the object to be created corresponds to the HTTP request body.

`GET /c/:app/:array?x=<JSON>`

`GET /c/:app/:array/:id?x=<JSON>`

Via `GET`, the object to be created needs to be passed as JSON string in the URL query param `x`.

> <a href='https://qi.do/c/test/order?x={"items":[3,5,8],"total":47.3,"comment":"deliver at door"}' target='_blank'>qi.do/c/test/order?x={"items":[3,5,8],"total":47.3,"comment":"deliver at door"} </a>

```typescript
// object to be created
const order = {
  items: [3, 5, 8],
  total: 47.3,
  comment: 'deliver at door'
}
// create object and log response
app.create('order', order)
  .then(data => console.log(data))
```

```javascript
// get your form element
const form = document.getElementById('order')
// convert to form data
const order = new FormData(form)
// create order along with all files
app.create('order', order)
  .then(data => console.log(data))
```

```html
<form id="order">
  <input type="text" name="items" placeholder="items">
  <input type="text" name="total" value="47.3">
  <input type="text" name="comment" value="deliver at door">
</form>
```

```shell
curl https://qi.do/c/test/order \
-d '{"items":[3,5,8],"total":47.3,"comment":"deliver at door"}' \
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
The JSON-encoded response contains the id of the newly created object.

### URL parameters

In the request URL, you must replace the parameters below with your respective data.

Parameter | Description | Required
--------- | ----------- |  -----------
app | The name of the app to be used. | true
array | The name of the array to be used. | true
id | The id of the object to be created. | false

### Query parameters

If you are using `GET`, the object to be created has to be passed as JSON string in the query param `x`.

Parameter | Description | Required
--------- | ----------- |  -----------
x | The object to be created. | true







## Read objects

The microservice `/r` allows you to read objects (documents/entries) that are stored in a specific array (collection/table).

### Read all objects

`GET /r/:app/:array`

Through the endpoints ending with `:array`, all objects in this array are retrieved.

> <a href="https://qi.do/r/test/order" target="_blank">qi.do/r/test/order </a>

```typescript
// read all objects from order array
app.read('order')
  .then(data => console.log(data))
```

```javascript
// read all objects from order array
app.read('order')
  .then(data => console.log(data))
```

```html

```

```shell
curl https://qi.do/r/test/order \
-H 'Content-Type: application/json' \
-H 'Authorization: Bearer token'
```
> HTTP response:

```json
[...] // all objects in the array
```

### Read specific objects

`GET /r/:app/:array?x=<JSON>&o=<JSON>`

In order to retrieve specific objects, you have to set a filter to the query param `x` in the request URL.
One can also **sort** and **paginate** queries by sending the options via query param `o`.
For more details about how to query objects with qi.do, please refer to the
<a href="https://mongodb.github.io/node-mongodb-native/markdown-docs/queries.html#query-object" target="_blank">MongoDB</a>
documentation.

> <a href='https://qi.do/r/test/order?x={"total":{"$gte":20}}&o={"limit":3}' target='_blank'>qi.do/r/test/order?x={"total":{"$gte":20}}&o={"limit":3} </a>

```typescript
// read specific objects from order array
const filter = {total: {$gte: 20}}
const options = {limit: 3}
app.read('order', filter, options)
  .then(data => console.log(data))
```

```javascript
// read specific objects from order array
const filter = {total: {$gte: 20}}
const options = {limit: 3}
app.read('order', filter, options)
  .then(data => console.log(data))
```

```html
```

```shell
curl https://qi.do/r/test/order?x={"total":{"$gte":20}}&o={"limit":3} \
-H 'Content-Type: application/json' \
-H 'Authorization: Bearer token'
```

> HTTP response:

```json
[
  {
    "_id": "5feca140530c0772b232d3e5",
    "items": [3, 5, 8],
    "total": 47.3,
    "comment": "deliver at door"
  },
  {
    "_id": "5fcbde89c5ef0493e50a2fc3",
    "items": [4, 6],
    "total": 23.5,
    "comment": "without peperoni"
  },
  {
    "_id": "5fcbdc6dc5ef0493e50a2fc0",
    "items": [4, 9],
    "total": 19.2
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

`GET /r/:app/:array/:id`

A single object can be retrieved by attaching its id to the request URL.

> <a href="https://qi.do/r/test/order/5fcbde89c5ef0493e50a2fc3" target="_blank">qi.do/r/test/order/5fcbde89c5ef0493e50a2fc3 </a>

```typescript
// read object by id
app.read('order/5fcbde89c5ef0493e50a2fc3')
  .then(data => console.log(data))
```

```javascript
// read object by id
app.read('order/5fcbde89c5ef0493e50a2fc3')
  .then(data => console.log(data))
```

```html

```

```shell
curl https://qi.do/r/test/order/5fcbde89c5ef0493e50a2fc3 \
-H 'Content-Type: application/json' \
-H 'Authorization: Bearer token'
```

> HTTP response:

```json
{
  "_id": "5fcbde89c5ef0493e50a2fc3",
  "items": [4, 6],
  "total": 23.5,
  "comment": "without peperoni"
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

The microservice `/u` allows you to update an object in an array.
A request can be sent through the HTTP methods `PUT` and `GET` (if enabled).

### HTTP endpoints

`PUT /u/:app/:array/:id`

Using `PUT`, the object data to be updated corresponds to the request body.

`GET /u/:app/:array/:id?x=<JSON>`

Via `GET`, the data needs to be passed as JSON string in the query param `x` of the URL.

> <a href='https://qi.do/u/test/order?x={"total":45,"paid":true}' target='_blank'>qi.do/u/test/order?x={"total":45,"paid":true} </a>

```typescript
// properties to be updated/added
const order = {
  total: 45,
  paid: true
}
// update object by id
app.update('order/5fcbdc57c5ef0493e50a2fbd', order)
  .then(data => console.log(data))
```

```javascript
// get your form element
const form = document.getElementById('order')
// convert to form data
const order = new FormData(form)
// update profile
app.update('order/5fcbdc57c5ef0493e50a2fbd', order)
  .then(res => console.log(res))
```

```html
<form id="order">
  <input type="text" name="total" value="45">
  <input type="checkbox" name="paid" checked>
</form>
```

```shell
curl https://qi.do/u/test/order/5fcbdc57c5ef0493e50a2fbd \
--request PUT \
--data '{"total":45,"paid":true}' \
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

If you are using `GET`, the properties to be updated/added have to be passed in the query param `x`.

Parameter | Description | Required
--------- | ----------- |  -----------
x | The properties to be updated/added. | true







## Delete an object

The microservice `/d` allows you to delete an object from an array.
A request can be sent through the HTTP methods `DELETE` and `GET` (if enabled).

### HTTP endpoints

`DELETE /d/:app/:array/:id`

`GET /d/:app/:array/:id`

Using both methods, it is just necessary to append the object id in the URL.

> <a href="https://qi.do/d/test/order/5fcbdeb1c5ef0493e50a2fc4" target="_blank">qi.do/d/test/order/5fcbdeb1c5ef0493e50a2fc4 </a>

```typescript
// delete object by id
app.delete('order/5fcbdeb1c5ef0493e50a2fc4')
  .then(data => console.log(data))
```

```javascript
// delete object by id
app.delete('order/5fcbdeb1c5ef0493e50a2fc4')
  .then(data => console.log(data))
```

```html

```

```shell
curl https://qi.do/d/test/order/5fcbdeb1c5ef0493e50a2fc4 \
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
