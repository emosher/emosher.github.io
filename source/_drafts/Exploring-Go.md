---
title: Exploring Go
tags: |-

  - go
  - google
  - syntax
  - opinion
permalink: exploring-go
id: 17
updated: '2014-08-19 18:55:31'
---

I've recently been reading and experimenting a bit with [Go](http://golang.org/). If you'd like to get familar with Go, I recommend taking a look at [the tour](http://tour.golang.org/#1) and the documentation on Go's website.

I've taken a look at the language and I am providing my opinion on some of its features here. 

### Background

Go is a language built by [Google](http://www.google.com). Go is used by Google to support their download site (dl.google.com), which serves downloads of Chrome and other Google products.

For more on who is using Go, see [this link](http://en.wikipedia.org/wiki/Go_(programming_language)#Notable_users).  

### Exploring the Syntax

The creators of Go made some interesting decisions when they created the language. I like the fact that they took the time to carefully choose each feature of the language and not include anything they felt to be unnecessary.

I may or may not agree with the decisions made in the development of Go, but I can appreciate the desire to create something the *right* way. Even if "right" is a subjective term.

#### For Loops

Go has `for` loops in a similar fashion to C, but does not include `while` loops. A typical for loop looks like the following.

```go
for i := 1; i < 10; i++ {
	// Do stuff
}
```

The tour I linked above states:
> The basic for loop looks as it does in C or Java, except that the ( ) are gone (they are not even optional) and the { } are required.

I see what they are trying to do - by cutting down on the number of parentheses used by the core syntax. This allows you (the programmer) to save parens for actual arithmetic statements. As someone that has written [Lisp](http://en.wikipedia.org/wiki/Lisp_(programming_language)) code, this is something I can get behind.

#### While Loops

Go has no `while` loop. However, the syntax of the `for` loop is flexible enough to accomodate a similar syntax. Rewriting the above for loop, we can do the following to create a similar structure to a while loop.

```go
i := 0
for i < 10 {
	// Do stuff
    i++
}
```