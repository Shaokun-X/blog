---
title: "Exceptions logging in python"
date: 2022-11-18T17:07:26+01:00
description: "List several ways of logging exception information in python."
tags: ["python"]
draft: false
---

There are several ways of logging exception information in Python:

```python
import sys, logging
l = logging.getLogger('test')
l.addHandler(logging.StreamHandler(sys.stdout))

def foo():
    try:
        raise KeyError("haha, an error")
    except Exception as e:
        l.error("%s sep %s", "hello", e, exc_info=True)
        print("--------")
        l.error("%s sep %s", "hello", e)
        print("--------")
        l.exception(e)
        print("--------")
        l.exception("", exc_info=e)
        print("--------")
        l.exception(e, exc_info=1)
        print("--------")
        print(str(e))
        print("--------")
        print(repr(e))
```

The output would be:

```shell
hello sep 'haha, an error'
Traceback (most recent call last):
  File "<ipython-input-1-3354851c0b12>", line 7, in foo
    raise KeyError("haha, an error")
KeyError: 'haha, an error'
--------
hello sep 'haha, an error'
--------
'haha, an error'
Traceback (most recent call last):
  File "<ipython-input-1-3354851c0b12>", line 7, in foo
    raise KeyError("haha, an error")
KeyError: 'haha, an error'
--------

Traceback (most recent call last):
  File "<ipython-input-1-3354851c0b12>", line 7, in foo
    raise KeyError("haha, an error")
KeyError: 'haha, an error'
--------
'haha, an error'
Traceback (most recent call last):
  File "<ipython-input-1-3354851c0b12>", line 7, in foo
    raise KeyError("haha, an error")
KeyError: 'haha, an error'
--------
'haha, an error'
--------
KeyError('haha, an error')
```

`logger.exception` is essentially a wrapper for `logger.error(exc_info=1)`. 