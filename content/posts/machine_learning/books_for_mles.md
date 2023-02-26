---
title: "Books for Machine Learning Engineers"
date: 2023-02-26
draft: false
series: "Machine Learning"
tags: ["machine-learning"]
---

This article will try to cover prominent textbooks that may be of interest to machine learning practitioners, with a focus on ML engineers. However, due to the applied mathematics nature of ML, we will include many texts that may be of interest to other engineers & researchers, especially in the non ML subsections.

This list is not comprehensive, and alternatives are proposed for some books. Not all books are perfect for all readers, and many other sources such as blog posts, online video lectures, or written lectures not in the form of books exist and should be leveraged alongside these texts.

Most book lists are sparse in information, which while making them easier to browse, makes it hard to contextualise why a certain book exists in the list. I try to motivate my reasons for including specific books and their alternatives.

Finally, it is impossible to read all of these textbooks, and many topics are not relevant for all readers. If you have any suggestions for alternative books or topics missing from below do reach out!

Do note that the links are referral links to Amazon, and other sellers do exist, but purchasing through these may give me a few cents.

## Applied Mathematics

Applied mathematics is at the core of scientific computing. We differentiate between applied mathematics and machine learning even though a lot of machine learning texts are basically applied mathematics textbooks. This is just to help simplify navigation.
The core applied mathematics topics that concern us, as ML practitioners, are numerical linear algebra, numerical methods, probability theory, probabilistic programming and optimization.

### Numerical Linear Algebra

Vector and matrix operations are present in almost of all of scientific computing (including ML). Being able to efficiently manipulate these as well as perform mathematical operations is one of the most important components of modern computing. A basic knowledge of linear algebra itself is recommended from texts such as [Introduction to Linear Algebra by Strang](https://amzn.to/3xQK8X4).

The numerical component of this deals with the algorithms & datastructures we have developed for representing and working with vectors and matrices. Some popular books are:
- [Matrix Computations](https://amzn.to/3xWfdZg)
- [Numerical Linear Algebra](https://amzn.to/3XXrEic)


### Numerical Methods

Numerical linear algebra covers some subsets of topics taught in numerical methods, such as numerical stability, however numerical methods exist for more than just working with matrices! Numerical solvers are at the heart of much of scientific computing, used for optimization purposes, solving differential equations and more!

A very popular book is [Numerical Recipes](https://amzn.to/3XZLDN7), it is however quite dated and some of the algorithms follow bad programming practices. It is nevertheless a great way to see and experience how numerical algorithms can be implemented.

For a more general text on numerical analysis, a commonly recommended textbook used in many undergraduate courses is [Numerical Methods for Scientists and Engineers](https://amzn.to/3kv0KAt).

A new book that I personally find interesting is [Probabilistic Numerics: Computation as Machine Learning](https://amzn.to/3KAqF4t) by University of Tubingen which discusses many core algorithms of numerical computing in a machine learning context, as well as ways their research groups are modernizing these (as well as introducing probabilistic versions of these algorithms).

### Probability Theory

### Probabilistic Programming

[Bayesian Data Analysis by Gelman](https://amzn.to/3EAY3UQ) is a core textbook used in most universities as a first introduction to Bayesian modelling from a computational perspective.

Another more recent simpler book is [Bayesian Modelling and Computation in Python](https://amzn.to/3Y1WmGL), which covers many applied uses of bayesian modelling. It is however more opinionated and therefore not personally my favorite text on the topic. A third simpler approach is [Statistical Rethinking](https://amzn.to/3kv1iGx), which also introduces the basics of statistical modelling. For most graduates I would still recommend Gelmans book.

### Optimization

### Advanced/Specialised Books

- Matrix Computations
- Markov Chain Monte Carlo
- High Dimensional Probability

## Computer Science

### Algorithms & Datastructures

### Design Patterns

[Design Patterns by the gang of four](https://amzn.to/3ZFAuT9) 

### System Design

Not many books focus solely on system design. I highly recommend seeking alternative sources such as companies blogs, conference talks, videos and papers discussing specific implementations. However there are a few texts that may be of interest.

[Data Intensive Applications](https://amzn.to/3Y5q4uu) is one of the few books that assumes little to no background knowledge on the subject matter, yet manages to cover it end to end in a considerable amount of depth. It also manages to do so without being a tough read! I highly recommend this book be on all engineers shelves.

Another popular new text is the [System Design Interview](https://amzn.to/3ZeuUH0). While its focus is helping you pass a system design interview, it does so by introducing high level topics. I would not use it as a sole source for learning system design, as it's extremely high level and interview focused, but it's still worth a read.

[Designing Machine Learning Systems](https://amzn.to/3Y49afK) is a new book by Chip Huyen, it's a fantastic perspective on ML system design that I highly recommend. Instead of focusing only on the technical aspects of designing such systems, Chip discusses stakeholder engagement and what solutions look like for more abstract problems with multiple goals. I highly recommend it! Though if the reader expects deep discussions on system design they may be disappointed.

### Distributed Systems

## General Books

- The Last Lecture
- Mythical Man Month
- Pragmatic Programmer

## Machine Learning

### Statistical Learning

Introduction to Statistical Learning

### Advanced/Specialised Books

- Elements of Statistical Learning
- Probabilistic Graphical Models

## Deep Learning