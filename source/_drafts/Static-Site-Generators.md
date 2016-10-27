---
title: Static Site Generators
tags: |-

  - static-site
  - node.js
permalink: static-site-generators
id: 21
updated: '2015-02-18 21:07:54'
---

I've been thinking of moving this blog to a static site. I haven't yet made any decisions, but I thought I would take a look at some of them here. I really like Ghost, but I currently host it in Digital Ocean which costs money. With a static site, I could move to [Github Pages](https://pages.github.com/) which would be free. Money vs. No-money seems like a good reason to think about a change.

### What Is It?

For this post, I'm defining a "static site generator" by my own criteria. The tool in question should meet the following criteria:

 * Create a static site based markdown files
 * Use Node.js 
 * Some pre-built themes
 * Some kind of plugin or extension system
 * Variety of possible templating engines
 * A way to integrate user comments, either built in or through a plugin (preferably with [Disqus](https://disqus.com/) which I use now)
 * Use only static files with no backend (deployable to Github Pages)

These are *my* criteria. If you choose to use one of these tools your criteria could be very different. For me, [markdown](http://daringfireball.net/projects/markdown/) is a great way to write. It is one of the things that drew me to Ghost in the first place. As for Node.js, I am currently developing with it which means I don't want to have to install some kind of other platform or language. 

### What Is It Not?

While looking through various posts and sites on this topic a few things came up that I don't want. Whatever tool I choose should not be:

 * A custom solution
 * A build tool like [Brunch](http://brunch.io/)
 * An inactive project
 * Coffeescript (I want JS)

Any solution I use should be quick. I don't want to take the time to build something myself and I don't want to configure something like Brunch to do it. Sure, Brunch *can* do static site generation. But it's purposes are more than static site generation. With the abundance of options I found, I figured I would rule out any possibilities like this. I don't want something that hasn't been touched in over a year. Last, I want something where I can deal with Javascript. The precense of Coffeescript is not a deal-breaker but I prefer to use JS.

### Many Choices

While I searched on this topic, I found [StaticGen](https://www.staticgen.com/). It has some nice filtering options and includes all the major static site generators that I looked at. This is definitely a great place to get started when considering a static site generator.

I'll cover the bigger, more popular options below by trying them out and sharing my thoughts.

#### Metalsmith

#### Hexo

#### Punch

#### Docpad