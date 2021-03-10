---
title: "Python MRO"
date: 2021-03-10T10:35:17+01:00
description: "A brief description"
tags: ["python"]
draft: False
---

Like 2 years ago I firstly learned about the python3 MRO(method resolution order), I though I pretty much got the hang of it, until I met this:

```python
class X:pass
class Y: pass
class Z:pass
class A(X,Y):pass
class B(Y,Z):pass
class M(B,A,Z):pass
```

Apprently the search order should be 
>M B Y Z A X Y Z

thus the MRO is supposed to be 
>M B Y Z A X

But when I tried it with code, HELL NO.

```python
M.__mro__ 
# output
(__main__.M,
 __main__.B,
 __main__.A,
 __main__.X,
 __main__.Y,
 __main__.Z,
 object)
```
I got totally confused, checked on the Internet, and true that python mro is decided by depth-first traversal algorithm. And this [website](https://data-flair.training/blogs/python-multiple-inheritance/) says that:
> First, the interpreter scans M. Then, it scans B, and then A-B first because of the order of arguments at the time of inheritance.
> 
> It scans Z later, after X and Y. The order is- X, then Y, then Z. This is because due to depth-first search, X comes first to A.

What? what kind of depth-first search this is? I drilled on this description, feeling that I had lost all my knowlege in CS, and finally I dropped it away.

A few days later, I decided to reflect on this on my own. And as you might have seen this is actually pretty simple, **if a class appears more than one time in the search result, we take the last appearence.**

So if we have this
>M B Y Z A X Y Z

The result should be
>M B ~~Y~~ ~~Z~~ A X Y Z