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

The numerical component of this deals with the algorithms & datastructures we have developed for representing and working with vectors and matrices. A popular book introducing the fundamentals is [Numerical Linear Algebra by Trefethen](https://amzn.to/3XXrEic).

A popular book discussing specifically the algorithms involved in vector & matrix manipulation is [Matrix Computations](https://amzn.to/3xWfdZg). It is not a good source for a first introduction, but a great reference and it's a lot of fun to try implementing the methods.

### Numerical Methods

Numerical linear algebra covers some subsets of topics taught in numerical methods, such as numerical stability, however numerical methods exist for more than just working with matrices! Numerical solvers are at the heart of much of scientific computing, used for optimization purposes, solving differential equations and more!

A very popular book is [Numerical Recipes](https://amzn.to/3XZLDN7), it is however quite dated and some of the algorithms follow bad programming practices. It is nevertheless a great way to see and experience how numerical algorithms can be implemented.

For a more general text on numerical analysis, a commonly recommended textbook used in many undergraduate courses is [Numerical Methods for Scientists and Engineers](https://amzn.to/3kv0KAt).

A new book that I personally find interesting is [Probabilistic Numerics: Computation as Machine Learning](https://amzn.to/3KAqF4t) by University of Tubingen which discusses many core algorithms of numerical computing in a machine learning context, as well as ways their research groups are modernizing these (as well as introducing probabilistic versions of these algorithms).

There exist several more advanced books such as [handbook of MCMC](https://amzn.to/41vPnc8) and [Monte Carlo Statistical Methods](https://amzn.to/3KHxXDG) which are also worth a look.

### Probability Theory

Randomness (or should we say; [pseudorandomness](https://en.wikipedia.org/wiki/Pseudorandomness)) is fundamental to many algorithms in scientific computing. While we don't require a deep theoretical understanding of [sigma algebras](https://en.wikipedia.org/wiki/%CE%A3-algebra), an appreciation for probability theory is highly recommended.

[Sheldon Ross](https://amzn.to/41s02Vc) offers an introduction to probability models, though the book is quite slow paced. His other book [First Course in Probability](https://amzn.to/3m3aCSp) is a bit faster paced and covers the fundamentals quickly in the first few chapters.

There exist many more advanced books such as [High Dimensional Probability](https://amzn.to/3Zq7kH8) by Vershynin that cover interesting topics in great depth. These are however not necessary for most.

### Probabilistic Programming

[Bayesian Data Analysis by Gelman](https://amzn.to/3EAY3UQ) is a core textbook used in most universities as a first introduction to Bayesian modelling from a computational perspective.

Another more recent simpler book is [Bayesian Modelling and Computation in Python](https://amzn.to/3Y1WmGL), which covers many applied uses of bayesian modelling. It is however more opinionated and therefore not personally my favorite text on the topic. A third simpler approach is [Statistical Rethinking](https://amzn.to/3kv1iGx), which also introduces the basics of statistical modelling. For most graduates I would still recommend Gelmans book.

### Optimization

From operational research to deep learning, optimization problems are everywhere! While the Numerical Methods text discussed in a section above covers some methods that may be used to approach optimization problems, we here will focus on two types: numerical optimization methods as well as convex problems.

The most popular books would be [convex optimization by Boyd](https://amzn.to/3Sw9p25) and [numerical optimization by Nocedal](https://amzn.to/3SDKp9q).

## Computer Science

Machine Learning Engineers are expected to not only have strong mathematical & machine learning fundamentals, but also a background in computer science, with a focus on production code & deployment.

### Algorithms & Datastructures

The most popular algorithms book is [Introduction to Algorithms by CLRS](https://amzn.to/3SwCUks). It's been used by MIT in their main undergraduate algorithms class [6.0006](https://ocw.mit.edu/courses/6-006-introduction-to-algorithms-fall-2011/) for years. It's a great book, however it is quite a difficult read and quite proof heavy. It's a fantastic introduction to the world of algorithms and allows readers to naturally move onto more advanced algorithm design topics.

A popular book more focused on application and less on theory is [The Algorithm Design Manual by Skiena](https://amzn.to/3EGy6Dv).

### Design Patterns

[Design patterns](https://en.wikipedia.org/wiki/Software_design_pattern) come in many flavours, the most popular of which is the OOP book [Design Patterns by the gang of four](https://amzn.to/3ZFAuT9). It gives names and abstracts patterns we all have encountered such as the strategy, factory & adapter patterns. It's a relatively light read and I highly recommend it.

There are sadly not many books covering design patterns, however I do recommend a video covering functional design patterns.

I also recommend learning from the gaming industry, which has evolved several interesting patterns that are highly applicable in scientific simulation contexts, such as [data-oriented design](https://en.wikipedia.org/wiki/Data-oriented_design). Some books covering this are [Game Programming Patterns](https://amzn.to/3m4MNtw) and [Data-Oriented Design](https://amzn.to/3SyAsd3). Some of these patterns gave rise to entity component systems such as [EnTT](https://github.com/skypjack/entt).

### System Design

Not many books focus solely on system design. I highly recommend seeking alternative sources such as companies blogs, conference talks, videos and papers discussing specific implementations. However there are a few texts that may be of interest.

[Data Intensive Applications](https://amzn.to/3Y5q4uu) is one of the few books that assumes little to no background knowledge on the subject matter, yet manages to cover it end to end in a considerable amount of depth. It also manages to do so without being a tough read! I highly recommend this book be on all engineers shelves.

Another popular new text is the [System Design Interview](https://amzn.to/3ZeuUH0). While its focus is helping you pass a system design interview, it does so by introducing high level topics. I would not use it as a sole source for learning system design, as it's extremely high level and interview focused, but it's still worth a read.

[Designing Machine Learning Systems](https://amzn.to/3Y49afK) is a new book by Chip Huyen, it's a fantastic perspective on ML system design that I highly recommend. Instead of focusing only on the technical aspects of designing such systems, Chip discusses stakeholder engagement and what solutions look like for more abstract problems with multiple goals. I highly recommend it! Though if the reader expects deep discussions on system design they may be disappointed.

### Distributed Systems

Distributed computing is becoming more and more prevalent as single node machines have stopped yielding significant computational gains over time, while interconnects and other messaging methods have seen strong performance improvements. This has naturally led to larger and more complex distributed systems, and therefore a larger ecosystem of software to help leverage these.

The book designing data intensive applications we mentioned earlier does cover some distributed design topics, however not many books cover how to design algorithms which optimally leverage distributed systems. Some classics exist such as [Distributed Algorithms by Lynch](https://amzn.to/3xRBojt) and more modern books such as [Distributed Algorithms bu Fokkink](https://amzn.to/3EFL4kH), however most sources are either in the form of conference talks or articles.


## Machine Learning

### Statistical Learning

Introduction to Statistical Learning
Machine Learning: A Probabilistic Perspective

### Advanced/Specialised Books

- Elements of Statistical Learning
- Probabilistic Graphical Models

## Deep Learning

## General Books

While textbooks are a fantastic resource for improving technical skills, understanding how to manage the workplace, work life balance and life in general is equally (if not more) important. I will now list some books that I found of great quality and important to read, however this list is obviously not comprehensive and readers may disagree with some choices.

- [The Last Lecture by Pausch](https://amzn.to/3EGAtWS) is written from the perspective of [Andy Pausch](https://en.wikipedia.org/wiki/Randy_Pausch), a talented software engineer who was sadly diagnosed with terminal cancer. He gave a final speech titled "Really Achieving Your Childhood Dreams", but referred to as "The Last Lecture", shortly before his death. This speech is the basis of the book.
- [Mythical Man Month](https://amzn.to/3Zsltnz) focuses on project management in software development drawing on the extensive experience of its author, [Fred Brooks](https://en.wikipedia.org/wiki/Fred_Brooks). It's a must read for anyone interested in software development, especially in larger scale organizations.
- [The Pragmatic Programmer](https://amzn.to/3Z97VgP) discusses what it really means to be a software engineer, covering topics ranging from personal & career development to navigating technical problems such as system design.
- [Gödel, Escher, Bach: An Eternal Golden Braid](https://amzn.to/41u1EOj) is a classic must read that discusses the emergent properties of simple systems and the meaning behind them, from the perspective of the mathematician Gödel, artist Escher and composer Bach. Warning, it's a longer read than the rest!