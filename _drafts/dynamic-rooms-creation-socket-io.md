---
layout: post
title:  "Joining dynamically generated rooms with Socket.io and React."
date:   2016-04-03 13:53:14 -0500
categories: JavaScript
excerpt: "Real time is the way of the future, and Socket.io is an amazingly easy way to start incorporating real time updates into an application.  Of course, just because it's relatively simple doesn't mean that everything is blatantly obvious."
sidebar: javaScript
---
Real time is the way of the future, and [Socket.io](socket.io) is an amazingly easy way to start incorporating real time updates into an application.  Of course, just because it's relatively simple doesn't mean that everything is blatantly obvious.
In this post, I'm going to demo a simple system to dynamically generate and join rooms in socket.io.  I'll be writing my front-end code for React and my back-end code for NodeJS with Express.
Say, for example, that you have an app where users can input events.  You want to use Socket.io to get real-time updates as users update an event's details and update their RSVP status.  You've read a few tutorials, so the basic setup seems fairly simple.

In our main server file, we'll set up and export the connection.

```javascript
// server.js
export const io = require('socket.io')(server);

io.on('connection', (socket) => {
  console.log('a user connected');
  socket.on('disconnect', () => {
    console.log('a user disconnected');
  });
});
```

With that done, we can call upon our socket connection from anywhere in our server.

```javascript
// some other file
import { io } from 'server';

// when some change occurs...
io.sockets.emit('updateEvent', data);
...
```

And from there, we can listen for the event on our client by importing socket.io-client.

```javascript
...
import io from 'socket.io-client';

class Events extends Component {

  componentWillMount() {
    this.socket = io();
    this.socket.on('updateEvent', (data) => {
      // do something with the data
    });
  }
...
```

Awesome!  But wait... no matter which event your user is looking at, they'll get piped updates for absolutely everything.  This doesn't seem very useful.  Fortunately, Socket.io gives you a way to filter 

On the front end, you can subscribe to a room named
```javascript
...
import io from 'socket.io-client';

class Ideas extends Component {

  componentWillMount() {
    this.socket = io();
    this.socket.on('connect', () => {
      this.socket.emit('subscribe', this.props.id);
    });
  }

  componentWillUnmount() {
    this.socket.emit('unsubscribe', this.props.id);
  }
...
```
