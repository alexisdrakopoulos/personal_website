---
title: "Annoying Python Patterns"
date: 2023-02-22
draft: false
series: "Python Review"
tags: ["python"]
---

This article will not cover spaghetti code or nasty one liners, but rather complain and discuss commonly found patterns which range from mildly annoying or unaesthetic all the way to outright dangerous. Patterns discussed here are ones I have come across in production code. Note that all production code has been abstracted away and may come from open-source software or industry code bases. Some patterns discussed might mainly occur in ML or scientific code bases, however most will be relevant to all Python programmers.

We do not offer solutions to each antipattern, but do try to discuss why these problems may occur. It's important to recognise that problems deeper in a code base, such as nasty funcs to handle edge cases with a dozen conditionals, are often a symptom of bad upstream design.

# Nones & NaNs

From optional args and returns to placeholder variables, None is the bane of clean code.

## could this arg be None? maybe...

Here is the worst example I have ever seen of what optional args can do to a code base:
```python
def multiply_potentially_nones(arg1: Optional[float] = None, 
                               arg2: Optional[float] = None) -> float:
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

We now come to the first dangerous pattern! Using conditional statements to abuse Pythons truethiness/falsiness of various datatypes. In case you aren't aware, here's the behavior that is abused:
```python
print(None or "hi there")
>>> hi there
```
In production this hides and looks like:
```python
x: dict = x or {}
```
which is often hidden tucked away in the return statement.

So why is this so bad?

Not only `None` is falsy!
```python
print([] or "hi there")
print("" or "hi there")
print({} or "hi there")
print(0 or "hi there")
print(0.0 or "hi there")
```
print statements are perhaps not the best way to highlight this, you can also do `bool([])`.

So doing:
```python
x: dict = x or {}
```
will `return {}` if x is any falsy value, rather than a None. This is extremely dangerous, for example imagine our previous func `multiply_potentially_nones`:
```python
def multiply_potentially_nones(arg1: Optional[float] = None, 
                               arg2: Optional[float] = None) -> float:
    arg1 = arg1 or 1.0
    arg2 = arg2 or 1.0
    return arg1*arg2

print(multiply_potentially_nones(5, None))
>>> 5.0 # wooo!
print(multiply_potentially_nones(5, 0.0))
>>> 5.0 # oh dear
```

# State Machines with Hidden State

This section will be a little longer and very ML specific, so feel free to skip it! I wanted it to be more in depth and give a proper example that is not extremely minimal.

Let's build a simple linear regression model using the handy least squares solver in the NumPy library:
```python
import numpy as np
from numpy.linalg import lstsq

class LinearRegression:
    def __init__(self):
        self.model = np.linalg.lstsq
    
    def fit(self, x: np.ndarray, y: np.ndarray) -> None:
        self.model = lstsq(x, y, rcond=None)[0]

    def predict(self, x: np.ndarray) -> np.ndarray:
        return np.dot(self.model, x)
```
We can now do 
```python
num_rows = 50
num_features = 3

x = np.random.uniform(size=[num_rows, num_features])*10
# sum of inputs + some noise to make it harder
y = np.sum(x, axis=1) + np.random.normal(size=num_rows, scale=5)

linear_regression = LinearRegression()
linear_regression.fit(x, y)
# output should be 1 + 2 + 3 = 6
linear_regression.predict(np.array([1, 2, 3]))
>>> 6.200 # on my machine
```
This is a common API followed by most popular ML toolkits, including the [scikit-learn estimators](https://scikit-learn.org/stable/developers/develop.html), [catboost](https://catboost.ai/en/docs/concepts/python-usages-examples), [xgboost](https://xgboost.readthedocs.io/en/stable/) & [lightgbm](https://lightgbm.readthedocs.io/en/v3.3.2/)!

### So what's the issue?