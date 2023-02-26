---
title: "Hidden State Machines"
date: 2023-03-01
draft: false
series: "Python AntiPatterns"
tags: ["python", "machine-learning"]
---

This article will be quite ML specific, so feel free to skip it! I wanted it to be more in depth and give a proper example that is not extremely minimal.

## Models

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

## So what's the issue?