---
layout: post
title:  "A Field Guide to Instantiation patterns"
date:   2016-04-03 11:02:00 -0500
categories: JavaScript
---

One of the biggest obstacles in learning about prototype chains in JavaScript is keeping track of all the ways you can make them.  This is by no means a complete explanation of the many forms and features of these functions, simply a quick visual reference and field guide to identifying them in the wild.

![A graphical representation of some instantiation patterns](https://raw.githubusercontent.com/fswiecki/fswiecki.github.io/master/_images/instantiation-patterns.png){: .center-img}

There are four instantiation patterns in JavaScript: __functional, functional-shared, prototypal__, and __pseudoclassical__.  The first and last are quite distinct from each other, but the first three and the last three share enough similatirites of construction to be slightly confusing and the middle two are almost identical in appearance.  

## Functional
the functional pattern, all properties and methods will be set within the constructor function. References to external objects are not necessary.  The keyword `this` is not needed to ensure that methods will apply to the instance being created and you are unlikely to encounter it.

## Functional-shared
Like the functional pattern, the functional-shared pattern creates an object 