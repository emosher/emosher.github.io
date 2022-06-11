---
title: Local Storage to Production
tags: |-

  - angularjs
  - javascript
permalink: local-storage-to-production
id: 19
updated: '2014-10-11 14:14:50'
date: 2014-09-20 23:29:37
---

Some of the javascript projects I've worked on never left the prototype phase, but others have made it into some form of production. Every once in a while, I encounter something that causes major ripple effects within my frontend code.

What I encountered recently was the difference between synchronous and asynchronous. While the difference is often obvious, it can be easy to overlook. 

### LocalStorage is Synchronous!

Enter localStorage. I sometimes see apps that are in the prototype phase using local storage as a way to store data. I usually shy away from this method but we recently gave it a shot in an AngularJS project I'm working.

This worked great for a while, until I decided to move to an async API. I then realized that [local storage is synchronous](http://stackoverflow.com/questions/20231163/is-html5-localstorage-asynchronous).

### Refactors Abound

This realization resulted in a massive refactor. It caused ripple effects *everywhere* because this app used it *everywhere*.

There are many [articles](http://kryogenix.org/days/2012/03/20/step-away-from-the-localstorage/) and [blogs](http://www.nczonline.net/blog/2012/03/07/in-defense-of-localstorage/) [about](http://oscargodson.com/posts/why-localstorage-has-already-failed-us.html) [this topic](http://www.webdirections.org/blog/localstorage-perhaps-not-so-harmful/).

There's plenty of information in those links so I leave it up to the reader to form their own opinion of localStorage.

### Keep Production in Mind

It seems to me that the most important part of using localStorage is to keep your end state in mind. What is your app going to do in production?

As developers, we're often guilty of taking a "get it working now" mentality. In this case, we took this to heart a bit too much and forgot our end state.

Personally, I see a few different uses for localStorage. However, there is no place for localStorage when you will later want to replace it with an async call.