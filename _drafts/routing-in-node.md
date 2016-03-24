---
layout: post
title:  "Exploring Routing in Node.js"
date:   2016-03-23 13:53:14 -0500
categories: JavaScript
excerpt: "Too many conditional statements?  Node server getting out of hand?  Objects may be the answer..."
sidebar: javaScript
---

One of the largest challenges I've faced in implimenting modular code as I design basic servers in Node is avoiding writing sprawling and messy conditional statements. As an easier to maintain and harder to break alternative, JavaScript's native object pattern is a great way to store information.  Pieces of data (in this case, function calls) can be organized by easily understood keys and retrieved trivially when they are needed. 

The first place this pattern pops up in designing a Node server is for handling the various HTTP requests that might arise.  Sure, you could write something like this:


```javascript
var sendResponse = function(response, data, status) {
  status = status || 200;
  response.writeHead(status, headers);
  response.end(data);
};

var handleRequest = function (request, response) {
  var action = actions[request.method];
  if (action === 'GET') {
    sendResponse(response, data, 200)
  } else if (action === 'POST') {
    sendResponse(response, data, 201)
  }else {
    helpers.sendResponse(response, 'something went wrong', 404);
  }
};

```

... but as your codebase grows more complex and as you add more requests (like `OPTIONS` or `PUT`) your `handleRequest` function will quickly become unruly.  What can we do instead?  We can create an `actons` object that stores functions to deal with every request we might want to handle.

```javascript
var actions = {
  GET: function(request, response) {
    sendResponse(response, data)
  },
  POST: function(request, response) {
    sendResponse(reponse, data, 201)
  }
};
```

This makes adding other handlers fairly trivial, and it's easy to see all of the options we have laid out so far.  The real bonus, though is that we can call this up with a single line of code in our `handleRequest` function.

```javascript
var handleRequest = function (request, response) {
  var action = actions[request.method];
  if (action) {
    action(request, response);
  } else {
    helpers.sendResponse(response, 'something went wrong', 404);
  }
};
```

The `handleRequest` function now looks to see if we have defined an action for the request method, and if we have, it runs it.  If we haven't created an appropriate response, we can default to an error message.

This pattern can also be applied to creating routers for a variety of URL paths that might be sent to our node server. 

```javascript
var routes = {
  '/': //a function,
  '/users': //a differenct function,
  '/favorites': //a third function
}
```

Once we have defined our routes in an object, it's as simple as calling up `routes[request.method]` to direct traffic to the appropriate place.  In practice, this might look something like the following:

```javascript
var route = routes[request.method];
if (route){
  route(request, response);
} else {
  sendResponse(response, 'something went wrong', 404)
}
```

And that's it!  Through the wonders of JavaScript objects, you can reign in your node server and keep your code concise, legible, and dry.