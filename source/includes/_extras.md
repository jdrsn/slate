# Permissions

### User authentication disabled

If your app has authentication disabled, then all objects in the app are public to anyone.
That means, any person (or robot) who knows how qi.do API works would be able to create, read, update and even delete objects from your app.

### Authentication via API key

However, if you protect your app with API keys, nobody would be able to execute your app's microservices except for those you give API keys.
That is very useful if third parties need access to your app.

### User authentication enabled

If your app has authentication enabled, every object automatically includes the user id that created that object.
This only means that every user on the app has access to all objects of all other users.
Not logged-in users do not have access to any data. 

In order to make any object private, you can set the property `_p` as follow:

Value | Permissions
--------- | -----------
<= 0 | Only own user can read/update/delete.
>= 1 | Other users can read.
>= 2 | Other users can read and update.
undefined | Other users can read/update/delete.







# Streams (real-time)

With qi.do, you can handle multiple real-time databases even in the same application.
For every array that you want to track changes, you must create a **stream** in the <a href="https://c.qi.do/streams" target="_blank">console</a>.

If you already have a <a href="https://developer.mozilla.org/en-US/docs/Web/API/Service_Worker_API/Using_Service_Workers" target="_blank">Service Worker</a> running on your app,
you just need to subscribe to your `stream`.

```typescript
// stream every change in the array
app.stream('message', '5fef00ae58daeb0b0b23edcb')
  .subscribe(stream => console.log(stream))
```

```javascript
// stream every change in the array
app.stream('message', '5fef00ae58daeb0b0b23edcb')
  .subscribe(stream => console.log(stream))
```

<br/>
Every time an object is created, updated or deleted, your app will instantly receive the information.





# Push notifications

It is very easy to push web notifications to users of qi.do apps.
A user needs to allow your app to send push subscriptions.
You can broadcast to any custom audience based on your subscriptions.

## Subscribe

The microservice `/p/<app>/s` allows you to create a push subscription object on an app.
This object can then be used to broadcast push notifications.
A request can be sent through the HTTP methods `POST` and `GET` (if enabled).

In order to be able to request a user to allow push notifications, you need your public **vapid** key.
This key can be found on the console in your app security settings.

More information about how to request permission for push notifications can be found <a href="https://developer.mozilla.org/en-US/docs/Web/API/Notifications_API/Using_the_Notifications_API" target="_blank">here</a>.

### HTTP endpoints

`POST /p/<app>/s`

Using `POST`, the data of the subscription to be created corresponds to the HTTP request body.

`GET /p/<app>/s?x=<JSON>`

Via `GET`, the subscription to be created needs to be passed as JSON string in the URL query param `x`.

> <a href='https://qi.do/p/test/s?x={"endpoint":"https://url...","keys":{"auth":"key","p256dh":"key"},"channel":"kids"}' target='_blank'>qi.do/p/test/s?x={"endpoint":"https://url...","keys":{"auth":"key","p256dh":"key"},"channel":"kids"} </a>

```typescript
// the subscription object
const subscription = {
  endpoint: 'https://url...',
  keys: {auth: 'key', p256dh: 'key'},
  channel: 'kids' // any custom audience
}
// subscribe to push notifications
app.subscribe(subscription)
  .then(res => console.log(res))
```

```javascript
// the subscription object
const subscription = {
  endpoint: 'https://url...',
  keys: {auth: 'key', p256dh: 'key'},
  channel: 'kids' // any custom audience
}
// subscribe to push notifications
app.subscribe(subscription)
  .then(res => console.log(res))
```

```shell
curl https://qi.do/p/test/s \
-d '{"endpoint":"https://url...","keys":{"auth":"key","p256dh":"key"},"channel":"kids"}' \
-H 'Content-Type: application/json' \
-H 'Authorization: Bearer token'
```

### URL parameters

In the request URL, you must replace `app` with your app name.

Parameter | Description | Required
--------- | ----------- |  -----------
app | The name of the app to be used. | true
s | The subscribe function. | true

### Query parameters

If you are using `GET`, the subscription to be created has to be passed as JSON string in the query param `x`.

Parameter | Description | Required
--------- | ----------- |  -----------
x | The subscription to be created. | true






## Broadcast

The microservice `/p/<app>/b` allows you to broadcast push notifications to a specific audience on an app.
A request can be sent through the HTTP methods `POST` and `GET` (if enabled).

You are able to broadcast push notifications to multiple users in a single request.

### HTTP endpoints

`POST /p/<app>/b`

`POST /p/<app>/b?q=<JSON>`

Using `POST`, the data of the notification to be created corresponds to the HTTP request body.

`GET /p/<app>/b?x=<JSON>`

`GET /p/<app>/b?x=<JSON>&q=<JSON>`

Via `GET`, the notification to be broadcasted needs to be passed as JSON string in the URL query param `x`.

If there is a custom audience, it should be passed in the query param `q` for both HTTP methods.

> <a href='https://qi.do/p/test/b?x={"title":"dad","body":"dinner is ready :)"}&q={"channel":"kids","_uid":{"$ne":"5feca140530c0772b232d3e5"}}' target='_blank'>qi.do/p/test/b?x={"title":"dad","body":"dinner is ready :)"}&q={"channel":"kids","_uid":{"$ne":"5feca140530c0772b232d3e5"}} </a>

```typescript
// the notifcation object
const notifcation = {
  title: 'dad',
  body: 'dinner is ready :)'
}
// the audience excluding own user id
const audience = {
  channel: 'kids',
  _uid: {$ne: '5feca140530c0772b232d3e5'}
}
// broadcast the notification to audience
app.broadcast(notifcation, audience)
  .then(res => console.log(res))
```

```javascript
// the notifcation object
const notifcation = {
  title: 'dad',
  body: 'dinner is ready :)'
}
// the audience excluding own user id
const audience = {
  channel: 'kids',
  _uid: {$ne: '5feca140530c0772b232d3e5'}
}
// broadcast the notification to audience
app.broadcast(notifcation, audience)
  .then(res => console.log(res))
```

```shell
curl https://qi.do/c/test/p/b?q={"channel":"kids","_uid":{"$ne":"5feca140530c0772b232d3e5"}} \
-d '{"title":"dad","body":"dinner is ready :)"}' \
-H 'Content-Type: application/json' \
-H 'Authorization: Bearer token'
```

### URL parameters

In the request URL, you must replace `app` with your app name.

Parameter | Description | Required
--------- | ----------- |  -----------
app | The name of the app to be used. | true
b | The broadcast function. | true

### Query parameters

If you are using `GET`, the notification to be broadcasted has to be passed as JSON string in the query param `x`.

Parameter | Description | Required
--------- | ----------- |  -----------
x | The notification to be broadcasted. | true
q | The audience of the broadcast. | false






# Mailer

It is also possible to send emails via qi.do.
However, our microservice for mail delivery is limited to SMTP only.
You can set up multiple SMTP configurations for every app.

## Send email messages

The microservice `/m/<app>` allows you to send email messages via SMTP.
A request can be sent through the HTTP methods `POST` and `GET` (if enabled).

We recommend creating SMTP configurations in the console.
However, you can also just pass a configuration as a parameter in the `mail` method (less secure).

### HTTP endpoints

`POST /m/<app>`

`POST /m/<app>/<id>`

`POST /m/<app>?q=<JSON>`

Using `POST`, the data of the email message to be sent corresponds to the HTTP request body.

`GET /m/<app>/<id>?x=<JSON>`

`GET /m/<app>?x=<JSON>&q=<JSON>`

Via `GET`, the message to be sent needs to be passed as JSON string in the URL query param `x`.

If you want to send through a custom SMTP config on the fly, in both HTTP methods it needs to be passed to the query param `q`.

> <a href='https://qi.do/m/test/5feca140530c0772b232d3e5?x={"to":"kids@example.com","sub":"dinner invitation","msg":"prepare to get fat"}' target='_blank'>qi.do/m/test/5feca140530c0772b232d3e5?x={"to":"kids@example.com","sub":"dinner invitation","msg":"prepare to get fat"} </a>

```typescript
// the notifcation object
const message = {
  to: 'kids@example.com',
  sub: 'dinner invitation',
  msg: '<p>prepare to get fat</p>'
}
// a custom config (not recommended)
let config = {
  host: 'smtp.gmail.com',
  port: 447,
  auth: {
      user: 'me@gmail.com',
      pass: 'p45ww0rd',
  }
}
// let config = '5fcbde89c5ef0493e50a2fc3'
// send message via config id or object
app.mail(message, config)
  .then(res => console.log(res))
```

```javascript
// the notifcation object
const message = {
  to: 'kids@example.com',
  sub: 'dinner invitation',
  msg: '<p>prepare to get fat</p>'
}
// a custom config (not recommended)
let config = {
  host: 'smtp.gmail.com',
  port: 447,
  auth: {
    user: 'me@gmail.com',
    pass: 'p45ww0rd',
  }
}
// let config = '5fcbde89c5ef0493e50a2fc3'
// send message via config id or object
app.mail(message, config)
  .then(res => console.log(res))
```

```shell
curl https://qi.do/m/test/5feca140530c0772b232d3e5 \
-d '{"to":"kids@example.com","sub":"dinner invitation","msg":"<p>prepare to get fat</p>"' \
-H 'Content-Type: application/json' \
-H 'Authorization: Bearer token'
```

### URL parameters

In the request URL, you must replace `app` with your app name.

Parameter | Description | Required
--------- | ----------- |  -----------
app | The name of the app to be used. | true
id | The id of the SMTP configuration. | false

### Query parameters

If you are using `GET`, the email message to be sent has to be passed as JSON string in the query param `x`.
The configuration param `q` can be set ẁith `POST` and `GET`.

Parameter | Description | Required
--------- | ----------- |  -----------
x | The email message to be sent. | true
q | The custom configuration object. | false
