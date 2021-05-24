---
layout: post
title: Graph coloring
gh-repo: daattali/beautiful-jekyll
tags: [algorithm, approximation]
comments: true
use_math: true
---

We need some definitions and theorems to discuss about the coloring.

### Definitions
A coloring of grapg $G$ is an assignment of colors to the vertices of $G$.
In this case, it can be considered as the assignemnts of sets like vertex 1 to set 1 and vetex 2 to set 3 and soon.

A graph is is $k$-coloring if each vertex is assigned to exactly one of $k$ colors.

A coloring is proper if no two adjacent vertices are assigned to the same color.

A graph is so-called $k$-colorable if it has a proper $k$-coloring.

### Theorems
It is known to be NP-gard to decide if a graph is 3-colorable.

It is known to be NP-gard to decide if a graph is 3-colorable or any of its proper coloring requires at least five colors.

{: .box-note}
**Reference** David P. Williamson and David B. Shmoys, The Design of Approximation Algorithms.