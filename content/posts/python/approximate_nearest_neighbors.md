---
title: "Approximate Nearest Neighbor Search"
date: 2023-02-27
draft: false
series: "Random"
tags: ["python", "mathematics"]
---

[Nearest neighbor search](https://en.wikipedia.org/wiki/Nearest_neighbor_search) refers to methods which try to find the "closest" $N$ neighbors to a certain point in space. To simplify this article lets assume a few things:
- We are in Euclidean space.
- We calculate distances using common distance metrics such as euclidean distance or angular distance.
- Our neighbors are N dimensional real vectors in Euclidean space.

The problem is therefore, for any given vector $v \in \mathbb{R}$ give us the $N$ nearest neighboring vectors.

The simplest way to do this is to compute the distance to every single other neighbor, and return the closest one. This might look like:
```python
import numpy as np

test_vectors = [np.array([1, 2, 3]), np.array([5, 10, 15]), np.array([-200, -200, -200])]
our_vector = np.array([0, 0, 0])

closest_vector_id = None
min_distance = float("inf")
for vector_id, vector in enumerate(test_vectors):
    distance = np.linalg.norm(vector - our_vector)

    if distance < min_distance:
        min_distance = distance
        closest_vector_id = 0

print(closest_vector_id)
>>> 0
```
So what's the issue? This seems fairly simple! This is an $O(n)$ time and $O(1)$ (extra) space algorithm and is very easy to understand, debug and test. The problem is that this requires performing this $O(n)$ computation each and every time a neighbor is wanted. Even a highly optimized version is not going to be ideal when dealing with large numbers of vectors ($10^6$ or more.)

Another solution that gives us $O(1)$ time and $O(n^2)$ space uses precomputation ($O(n^2)$ time) to compute every single pair of vectors distances allows us to find nearest neighbors efficiently. That would look like:
```python
vectors = [np.array([1, 2, 3]), np.array([5, 10, 15]), np.array([-200, -200, -200]), np.array([0, 0, 0])]

N = len(vectors)

# initialize the distance matrix to zero
distance_matrix = np.zeros(size=[N, N])

# perform O(n^2) precomputation
for i, vec_i in enumerate(vectors):
    for j, vec_j in enumerate(vectors):
        distance_matrix[i][j] = np.linalg.norm(vec_i - vec_j)

# we can now search quickly

```

## Popular Methods

### Annoy

### Faiss

### Redis