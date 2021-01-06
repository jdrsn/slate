# Streams (real-time)

You can have multiple fully real-time databases with qi.do.
For every array that you want to track changes, you must create a **stream** in the <a href="https://c.qi.do/streams" target="_blank">console</a>.

If you already have a service worker setup up and running on your app, you just need to `subscribe` to your `stream`.

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

For more information, refer to MongoBD's change streams.





# Push notifications

It is very easy to push web notifications to users of qi.do apps.
A user needs to allow your app to send push subscriptions.
You can broadcast to any custom audience based on your subscriptions.

## Subscribe

The operation `/p/<app>/s` allows you to create a push subscription object on an app.
This object can then be used to broadcast push notifications.
A request can be sent through the HTTP methods `POST` and `GET` (if enabled).

In order to be able to request a user to allow push notifications, you need your public **vapid** key, which can be found on the console in your app security settings.
More information about how to permission request for push notifications can be found here.

### HTTP endpoints

`POST /p/<app>/s`

Using `POST`, the data of the subscription to be created corresponds to the HTTP request body.

`GET /p/<app>/s?x=<JSON>`

Via `GET`, the subscription to be created needs to be passed as JSON string in the URL query param named `x`.

> <a href='https://qi.do/p/chat/s?x={"endpoint":"https://url...","keys":{"auth":"key","p256dh":"key"},"channel":"kids"}' target='_blank'>qi.do/p/chat/s?x={"endpoint":"https://url...","keys":{"auth":"key","p256dh":"key"},"channel":"kids"} </a>

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
curl https://qi.do/p/chat/s \
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

If you are using `GET`, the subscription to be created has to be passed as JSON string in the query param named `x`.

Parameter | Description | Required
--------- | ----------- |  -----------
x | The subscription to be created. | true






## Broadcast

The operation `/p/<app>/b` allows you to broadcast push notifications to a specific audience on an app.
A request can be sent through the HTTP methods `POST` and `GET` (if enabled).

You are able to broadcast push notifications to multiple users in a single request.

### HTTP endpoints

`POST /p/<app>/b`

Using `POST`, the data of the notification to be created corresponds to the HTTP request body.

`GET /p/<app>/b?x=<JSON>`

Via `GET`, the notification to be broadcasted needs to be passed as JSON string in the URL query param named `x`.
If there is a custom audience, it should be passed in the query param `q`.

> <a href='https://qi.do/p/chat/b?x={"title":"dad","body":"dinner is ready :)"}&q={"channel":"kids","_uid":{"$ne":"5feca140530c0772b232d3e5"}}' target='_blank'>qi.do/p/chat/b?x={"title":"dad","body":"dinner is ready :)"}&q={"channel":"kids","_uid":{"$ne":"5feca140530c0772b232d3e5"}} </a>

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
curl https://qi.do/c/chat/p/b?q={"channel":"kids","_uid":{"$ne":"5feca140530c0772b232d3e5"}} \
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

If you are using `GET`, the notification to be broadcasted has to be passed as JSON string in the query param named `x`.

Parameter | Description | Required
--------- | ----------- |  -----------
x | The notification to be broadcasted. | true
q | The audience of the broadcast. | false






# Mailer

## Send email messages

The operation `/m/<app>` allows you to send email messages via SMTP.
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

Via `GET`, the message to be sent needs to be passed as JSON string in the URL query param named `x`.

If you want to send through a custom SMTP config on the fly, it needs to be passed to the query param `q` in both HTTP methods.

> <a href='https://qi.do/m/chat/5feca140530c0772b232d3e5?x={"to":"kids@example.com","sub":"dinner invitation","msg":"come downstairs now!"}' target='_blank'>qi.do/m/chat/5feca140530c0772b232d3e5?x={"to":"kids@example.com","sub":"dinner invitation","msg":"come downstairs now!"} </a>

```typescript
// the notifcation object
const message = {
  to: 'kids@example.com',
  sub: 'dinner invitation',
  msg: '<p>come downstairs now!</p>'
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
  msg: '<p>come downstairs now!</p>'
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
curl https://qi.do/m/chat/5feca140530c0772b232d3e5 \
-d '{"to":"kids@example.com","sub":"dinner invitation","msg":"<p>come downstairs now!</p>"' \
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

If you are using `GET`, the email message to be sent has to be passed as JSON string in the query param named `x`.
The configuration param `q` can be set ẁith `POST` and `GET`.

Parameter | Description | Required
--------- | ----------- |  -----------
x | The email message to be sent. | true
q | The custom configuration object. | false
