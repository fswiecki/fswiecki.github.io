---
layout: post
title:  "Building a web app... tales from the front"
date:   2016-03-25 13:53:14 -0500
categories: JavaScript
excerpt: "I spent the last 48 hours building a tiny web-app.  These are my takeaways."
sidebar: javaScript
---

Over the last 48 hours, I put some of my programming skills to the test by building a very tiny web app.  And when I say 48 hours, I mean this in the loosest way possible.  The actual working time was closer to a mere 8 hours.  Clearly, there were corners to be cut and sacrifices to be made.  My big takeaways:

1. Scoping is your friend
  
  Choosing a realistic project scope is incredibly important.  It's easier to scale up than down! I decided to build a variation on a list app. At simplest I only needed to be able to input text and render it on the page, but I'd have plenty of room to add features and refinements.  Because I started on the small end, I had room to experiment with a variety of options once I had the bare minimum built.

2. Commit early and often

  Even (perhaps _especially_) when you're practicing, it's important to keep a good Git workflow!  Time got away from me at least once when I was working and I was shocked to see how many files I'd managed to change since my last commit.  Frequent commits will force you to focus your workflow and will give you the freedom to back up easily if you manage to get something really wrong.

3. Someone already built that
  
  When you're trying to do a basic task in JavaScript, chances are that somebody else has already encountered that task and already built that library.  I wanted to provide human-readable dates and do some basic calendar math for my app.  Instead of trying to create that functionality from scratch, I used [moment.js](http://momentjs.com/) to do the work for me.

4. Someone already built that (part 2)
  
  If you have an idea for an app, there's a pretty good chance that somebody else had that idea too and built it first.  That's fine.  If you're building an app for practice, it really doesn't matter how many to-do apps exist... they're still a great entry point to the world of app design.

5. Testing is good
  
  Test your app yourself.  Test your app on your friends.  Test your app on your mother.  It can be incredibly useful to get non-technical feedback from an average user.  They'll notice different problems than you!  It's easy to get distracted by your codebase and miss user experience bugs, like a form that doesn't work in Firefox or an incomplete delete button.