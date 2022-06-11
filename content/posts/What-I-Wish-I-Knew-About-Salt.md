---
title: What I Wish I Knew About Salt
date: 2016-07-02 19:10:39
tags: 
  - salt
draft: true
---

Over the past several months I have had the opportunity to dig into Salt (or SaltStack). I use it for everything from configuration and deployment of servers, to everyday troubleshooting. This post is a collection of short tidbits that I wish I knew about Salt when I started. This isn't meant to be an intro or how to about Salt. For that, I highly recommend the [tutorials in the Salt docs](https://docs.saltstack.com/en/latest/).

### Remote Execution
Salt is at its core, a remote execution engine. At first this seems like an "Ok, so what?" statement. If you are looking at Salt as a purely configuration management solution, this may not be readily apparent (it wasn't for me). So what is remote execution? It is the ability to execute some code on any number of machines and get the results back. This becomes *extremely* useful in debugging because you can do any shell-fu you want. Let's look at an example.

Say you have a set of Salt managed Linux machines and you want to look for a particular entry in `/etc/hosts`. If you were logged into a single Linux machine, you might run `cat /etc/hosts`. But you might want to be more specific. You might want to look for the localhost entry with `cat /etc/hosts | grep localhost`. With Salt you can check this across all connected machines.

```bash
salt '*' cmd.run 'cat /etc/hosts | grep localhost'
```

This really is the basics of Salt, because on top of this core functionality you can build a configuration management system (or any of the other Salt features).

### States vs. Modules

One of the things that was not immediately obvious to me was the differences between a module and a state. Let's start with basic example. You might run a command like this to install nginx.

```
salt '*' pkg.install nginx
```

In this case, you have run a module (and function) called [pkg.install](https://github.com/saltstack/salt/blob/develop/salt/modules/aptpkg.py#L433). On the other hand you might have a basic state such as this.

```yaml
# nginx.sls
nginx:
  pkg.installed
```

When you run this, you will do something like `salt '*' state.apply nginx`. Here's where it can get confusing. When you run this, you are running a state but also running a module. The state is `nginx` as defined above, but the module is `state.apply`. It is a subtle difference but it it useful to understand.

### Jinja Whitespace Control
This first one isn't so much about Salt as it is Jinja. Jinja has whitespace control built in. You might have a Jinja template that will render with a for loop and you might discover that the lines of the for loop will be rendered in the file as well. This is a good case for Jinja's whitespace control. It is not perfect but you will find that it will help make your templated config files a bit more tidy.  

```
exampel here
```


#### Ideas
 - remote execution
 - whitespace control
 -
