---
title: "Annoying Python Patterns"
date: 2023-02-18
draft: false
series: "Python Review"
tags: ["python"]
---

This article will not cover spaghetti code or nasty one liners, but rather complain and discuss commonly found patterns which range from mildly annoying or unaesthetic all the way to outright dangerous. Patterns discussed here are ones I have come across in production code. Note that all production code has been abstracted away and may come from open-source software or industry code bases. Some patterns discussed might mainly occur in ML or scientific code bases, however most will be relevant to all Python programmers.

We do not offer solutions to each antipattern, but do try to discuss why these problems may occur. It's important to recognise that problems deeper in a code base, such as nasty funcs to handle edge cases with a dozen conditionals, are often a symptom of bad upstream design.

# Nones & NaNs

From optional args and returns to placeholder variables, None is the bane of clean code.

## could this arg be None? maybe...

Here is the worst example I have ever seen of what optional Nones can do to a code base:
```python
def multiply_potentially_nones(arg1: Optional[float] = None, arg2: Optional[float] = None):
    if arg1 is not None and arg2 is not None:
        return arg1*arg2
    if arg1 is None and arg2 is not None:
        return arg2
    if arg1 is not None and arg2 is None:
        return arg1
```
While this could be simplified to something like:
```python
arg1 = arg1 if arg1 is not None else 1.0
arg2 = arg2 if arg2 is not None else 1.0
return arg1*arg2
```
which is mildly cleaner, it's still painful to read and quite hacky.

## if not None

This brings us to the 

```python
x = some_func(x) if x is None else x
```

## or

We now come to the first dangerous pattern! Using conditional statements to abuse Pythons truethiness/falsiness of various datatypes.
```python
print(None or "hi there")
print(None or "hi there")
print(None or "hi there")
print(None or "hi there")
print(None or "hi there")

```


```python
x: dict = x or {}
```