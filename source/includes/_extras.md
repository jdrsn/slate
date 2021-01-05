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

### Subscribe

In order to request push subscription to a user, you need your public vapid key.
This key can be found in your app security settings, in the console.

Assuming you have requested subscription, you just need to save it on the app.

```typescript
// the subscription object
const subscription = {
  endpoint: String,
  keys: [Object],
  channel: 'kids' // any custom param
}
// subscribe to push notifications
app.subscribe(subscription)
  .then(res => console.log(res))
```

```javascript
// the subscription object
const subscription = {
  endpoint: String,
  keys: [Object],
  channel: 'kids' // any custom audience
}
// subscribe to push notifications
app.subscribe(subscription)
  .then(res => console.log(res))
```

### Broadcast

You are able to broadcast push notifications to multiple users in a single request.

```typescript
// the notifcation object
const notifcation = {
  title: 'dad',
  body: 'dinner is ready :)'
}
// the audience excluding own user id
const audience = {
  channel: 'kids',
  _uid: {$ne: user._uid}
}
// broadcast the notification to audience
app.broadcast(notifcation)
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
  _uid: {$ne: user._uid}
}
// broadcast the notification to audience
app.broadcast(notifcation)
  .then(res => console.log(res))
```

# Mailer

You can also send e-mail via SMTP.
We recommend creating SMTP configurations in the console.
But you can also just pass a configuration into the API.

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
