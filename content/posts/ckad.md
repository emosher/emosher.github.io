---
title: "Ckad"
date: 2022-09-15T11:05:33-04:00
draft: false
---

# How I Got CKAD

[CKAD](https://www.cncf.io/certification/ckad/) is Certified Kubernetes Application Developer, which is a cerfication from the Cloud Native Computing Foundation ([CNCF](https://www.cncf.io/)).  I got mine in March of this year, and I had a couple folks ask about my experience so I thought I'd put together some thoughts about it.  When I started work on CKAD, I was brand new to Kubernetes.  If you've got experience with Kubernetes then you already have a head start!

Here's my best ideas on things that worked in getting CKAD the first time around.

### Start with a Course

I started my CKAD journey with a [course from A Cloud Guru](https://acloudguru.com/course/certified-kubernetes-application-developer-ckad).  This was a fantastic baseline for getting ready.  It has hands-on labs where you can get experience with the CLI and problems that mirrors the exercises on the exam.  It covers all the big topics that you might encounter on the exam.  If you decide to take a course from somewhere else - make sure it's hands-on!

### Look for Exercises

You can find several places with free exercises to help you prep for CKAD, many of them on GitHub [like this one](https://github.com/dgkanatsios/CKAD-exercises).  Run through a few of these and don't cheat!  Use only the Kubernetes docs and the basic `kubectl` CLI that you will have on the exam.  These are a decent measure for how prepared you are.

### Don't Wait Forever

You get 2 tries on the exam.  If you fail the first time, you get a free retake.  When I did mine, I wasn't sure I would pass and figured that worst case I could take it again with more preparation.  Turns out I passed, so I never needed that retake.  

### Day Of Tips

Here's some tips for the day of the exam:
- Don't get stuck, complete as many questions as possible
- Keep an eye on time and prioritize your time left
- Spend some time navigating the docs beforehand, since they are your only reference

And here's some CLI tips, that I found useful on the exam.
1. Know how to use both declarative and imperative operations.  E.g. `kubectl create -f nginx.yaml` or `kubectl run nginx --image=nginx`
1. Use dry run for generating YAML.  `kubectl create namespace my-ns --dry-run=client -o yaml > ns.yaml`
1. Use `kubectl run` for quickly creating a pod
1. The [Kubectl Cheat Sheet](https://kubernetes.io/docs/reference/kubectl/cheatsheet/) has some other goodies as well

### And Beyond

For me, CKAD was a great intro into the basics of Kubernetes constructs, but I've learned a lot since then.  I hope that this helps others that are getting into Kubernetes with CKAD!  
