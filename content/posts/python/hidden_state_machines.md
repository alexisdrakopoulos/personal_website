---
title: "Hidden State Machines"
date: 2023-02-26
draft: false
series: "Python AntiPatterns"
tags: ["python", "machine-learning"]
---

This article will be quite ML specific, though this issue is prevalent across most of scientific computing. We posit that a lot of scientific computing model classes are in fact [finite state machines](https://en.wikipedia.org/wiki/Finite-state_machine)! My goal is not to convince you that this has some deeper implication, but rather the annoyances of having to deal with objects of which certain attributes/methods are only valid when the object is in a specific state.

## Models

Let's build a simple [linear regression](https://en.wikipedia.org/wiki/Linear_regression) model using the handy [least squares](https://en.wikipedia.org/wiki/Least_squares) solver in the [NumPy](https://numpy.org/) library:
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

    def __len__(self):
        return len(self.model)
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

## So what's the issue?

We implicitly assume that our objects will be used in a specific sequential manner, with `fit` only being called after initialization, and `predict` only being called after `fit`.  If one were to try to call `predict` prior to `fit` they'd be faced with an error! We also assume that `len` is only called once the model has been `fit`.

Let's draw this out in a graph!
{{< mermaid >}}
flowchart LR
    y("Uninitialized Model") --> |"`__init__`"| h("Initialized Model")
    h --> |"fit()"| r("Fit Model")
    r --> |"fit()"| r
    r --> |"predict()"| su[/"Prediction"/]
    r --> |"__len__()"| suf[/"int"/]
{{< /mermaid >}}
