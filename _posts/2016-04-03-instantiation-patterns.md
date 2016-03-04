---
layout: post
title:  "A Field Guide to Instantiation patterns"
date:   2016-03-03 11:02:00 -0500
categories: tinkering
---

One of the biggest obstacles in learning about prototype chains in JavaScript is keeping track of all the ways you can make them.  This is by no means a complete explanation of the many forms and features of these functions, simply a quick visual reference and field guide to identifying them in the wild.

![A graphical representation of some instantiation patterns](fswiecki.github.io/_images/instantiation-patterns.png){: .center-img}

There are four instantiation patterns in JavaScript: __functional, functional-shared, prototypal__, and __pseudoclassical__.  The first and last are quite distinct from each other, but the first three and the last three share enough similatirites of construction to be slightly confusing and the middle two are almost identical in appearance.  
