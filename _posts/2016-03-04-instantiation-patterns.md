---
layout: post
title:  "A Field Guide to Instantiation patterns"
date:   2016-03-04 11:00:00 -0500
categories: JavaScript
---

One of the biggest obstacles in learning about object constructors in JavaScript is keeping track of all the ways you can make them.  This is by no means a complete explanation of these functions, simply a quick visual reference and field guide to identifying them in the wild.

![A graphical representation of some instantiation patterns](https://raw.githubusercontent.com/fswiecki/fswiecki.github.io/master/_images/instantiation-patterns.png){: .center-img}

There are four instantiation patterns in JavaScript: __functional, functional-shared, prototypal__, and __pseudoclassical__.  The first and last are quite distinct from each other, the first three and the last three share enough similarities of construction to be slightly confusing, and the middle two are almost identical in appearance.  

### Functional
![A graphical representation of functional instantiation](https://raw.githubusercontent.com/fswiecki/fswiecki.github.io/master/_images/functional.png)

__Key Identifiers__

  - function begins by creating an object and ends by returning that object 
  - all methods are stored within the constructor function 
  - use pattern: `var cat1 = catMaker("Princess", "white");` 

<hr class="clear">

The functional pattern constructor begins by creating an object instance and ends by returning that instance.  In the functional pattern, all properties and methods will be set within the constructor function. References to external objects are not necessary.  The keyword `this` is not needed to ensure that methods will apply to the instance being created.

```javascript
var catMaker = function(name, color){
  var anotherCat = {};
  var name = name;
  var color = color;
  anotherCat.meows = function(){
    console.log(name + " meows.");
  };
  anotherCat.sayHi = function(){
    console.log(name + " is a " + color + " cat.");
  }
  return anotherCat;
};
```

### Functional-shared
![A graphical representation of functional-shared instantiation](https://raw.githubusercontent.com/fswiecki/fswiecki.github.io/master/_images/functional-shared-prototypal.png) 

__Key Identifiers__

  - function begins by creating an object and ends by returning that object
  - all methods are stored outside of the constructor function
  - an `extend` function is used to give the newly created object access to the appropriate methods.
  - the keyword `this` is used in methods to refer to the appropriate object
  - use pattern: `var cat2 = catMaker("Tiger", "orange")`

<hr class="clear">

Like the functional pattern, the functional-shared pattern constructor begins by creating an object instance and returns the instance at the end. However, only the properties are present within the constructor function, and they are assigned directly to the object instance:

```javascript
var catMaker = function(name, color){
  var anotherCat = {};
  anotherCat.name = name;
  anotherCat.color = color;
...
```

Methods are stored on another object, either externally (i.e. `catMethods`) or on a property of the instantiation function (i.e. `catMaker.catMethods`) , and some sort of `extend` function is used to copy them to the instance in the constructor function before the object instance is returned.

```javascript
...
  extend(anotherCat, catMethods);
  return anotherCat;
}
```

Because the methods are stored on another object (which is not our newly created object instance), the keyword `this` is necessary to ensure that variable lookups go to the appropriate object.

```javascript
var catMethods = {};
catMethods.meows = function(){
  console.log(this.name + " meows.");
};
catMethods.sayHi = function(){
  console.log(this.name + " is a " + this.color + " cat.");
}
```
  
### Prototypal
![A graphical representation of prototypal instantiation](https://raw.githubusercontent.com/fswiecki/fswiecki.github.io/master/_images/functional-shared-prototypal.png) 

__Key Identifiers__

  - function begins by creating an object and ends by returning that object
  - all methods are stored outside of the constructor function
  - a prototypal relationship is established with `Object.create()`
  - the keyword `this` is used in methods to refer to the appropriate object
  - use pattern: `var cat3 = catMaker("Fluffy", "grey");`
      
<hr class="clear">

On the surface, prototypal instantiation looks very similar to functional-shared instantiation. All the important pieces are organized in the same way, but `Object.create()` is used to create a prototypal lookup for the methods instead of just extending the object instance to contain them.

```javascript
var catMaker = function(name, color){
  var anotherCat = Object.create(catMethods);
...
```

The actual location of the methods is unimportant, so some choose to store them in the `.prototype` or another named property (i.e. `.catMethods`) of their function.

```javascript
var catMaker = function(name, color){
  var anotherCat = Object.create(catMaker.prototype);
...
```


### Pseudoclassical
![A graphical representation of pseudoclassical instantiation](https://raw.githubusercontent.com/fswiecki/fswiecki.github.io/master/_images/pseudoclassical.png) 

__Key Identifiers__

  - properties declared with `this`
  - no return statement inside constructor function
  - methods stored on the `.prototype` property
  - usage pattern: `var cat4 = new catMaker("Mittens", "tabby");`
     
<hr class="clear">

The first thing you will probably notice about a pseudoclassical constructor function is that it's a few lines of code shorter.  Because pseudoclassical instantiation relies on the `new` keyword, it is unnecessary to write out the creation of a new object instance inside the constructor function -- JavaScript will take care of that for us.  Since there is no longer a named variable for the instance being worked on, the keyword `this` will need to be employed for property creation.

```javascript
var catMaker = function(name, color){
  this.name = name;
  this.color = color;
};
```

  Methods for pseudoclassical constructors *must* be stored on the `.prototype` property of the constructor in order to be found.
  
```javascript
catMaker.prototype.meows = function(){
  console.log(this.name + " meows.");
};
catMaker.prototype.sayHi = function(){
  console.log(this.name + " is a " + this.color + " cat.");
};
```

