---
layout: post
title:  "Binding and Currying Functions with Javascript"
date:   2016-03-29 22:55:14 -0500
categories: JavaScript
excerpt: "The function `bind` is a standard part of the JavaScript repertoire, so understanding how it works and being able to re-impliment it in a pinch is a good trick for a developer to have up his or her sleeve.  In this post, I'll explore basic context binding as well as supplying extra arguments to the bound function at various points along the way."
sidebar: javaScript
---

The function `bind` is a standard part of the JavaScript repertoire, so understanding how it works and being able to re-impliment it in a pinch is a good trick for a developer to have up his or her sleeve.  In this post, I'll explore basic context binding as well as supplying extra arguments to the bound function at various points along the way.

The typical completely unrealistic use case looks something like the following:

```javascript
var englishWords = {greeting: "Hello"}

var sayHello = function(name){
  console.log(this.greeting + " " + name);
}

sayHello('world') // -> "undefined world"
englishWords.sayHello('world') // -> error: englishWords.sayHello is not a function
```

We have a function we can use to say hello to some passed in string, and a (very small) dictionary of English words.  Unfortunately, we can't use the two together because `sayHello` is looking for some sort of `this` context to execute in, but `englishWords` doesn't actually have a `sayHello` method, so some sort of middleman is needed.

This is where `bind` comes in.

![A graphical representation of the purpose of the 'bind' function in JavaScript](/images/function-binding.jpg){: .center-img}

That seems fairly straightforward.  Let's write a basic binding function!  We'll take in the function and a context, and return a function that uses that context.  With this new function, we should be able to successfully call our `sayHello` function in the context of `englishWords`.  In fact this will be super useful anywhere we need to use a function somewhere that its context is likely to be lost.


```javascript
var bind = function(func, context){
  return function(){
    return func.apply(context);
  }
};  
```

Great!  Bind will now return a function that runs our function in our context. But we don't really have any way to deal with other arguments... what if we need to call our function in a context *and* pass in arguments?  We'll have to grab the arguments that are passed in at calltime and apply them along with the context.  We can just nab the arguments object, convert it into a real array, and pass it along.

But wait!  Won't `arguments` refer to the arguments passed into bind?  Turns out that won't be the case.  Thanks to the wonders of scoping, if we use `arguments` within the function that is returned (but not invoked!) by calling `bind`, we will be capturing the arguments passed in at calltime for that returned function.


```javascript
var bind = function(func, context){
  return function(){
    var calltimeArgs = Array.prototype.slice.call(arguments);
    return func.apply(context, calltimeArgs);
  }
};  
```

Awesome.  But... what if I want to pre-select some arguments when I initially call `bind`?  Perhaps I have some other arguments that will always be the same when used with this specific context.  In this case, I'll want to capture any arguments passed in to my bind function after the function and the context. Enter `arguments`... again.  This time we'll ask for `arguments` in the outer function scope to collect the arguments passed to `bind`, and then convert `arguments` into a real array and `slice` off the first two values (we already know that those are our function and our context).  Then we can store the rest of the arguments on a variable and pass them in before the calltime arguments.


```javascript
var bind = function(func, context){
  var args = Array.prototype.slice.call(arguments, 2)
  return function(){
    var calltimeArgs = Array.prototype.slice.call(arguments);
    return func.apply(context, args.concat(calltimeArgs));
  }
};  
```

Excellent!  Our bind function, for the purposes of this blog post, is done!  Bonus step: if we wanted to attach it as a method of the prototype of the Function object (for academic curiosity or polyfill purposes) we could.  The only extra step is saving the context that `bind` was called in (the function object that we are calling the `bind` method on) and making sure that we save it on a variable for later out-of-context use.

```javascript
Function.prototype.bind = function(context) {
  var func = this;
  var args = Array.prototype.slice.call(arguments, 1)
  return function(){
    var calltimeArgs = Array.prototype.slice.call(arguments);
    return func.apply(context, args.concat(calltimeArgs));
  }
};
```

And that's it!