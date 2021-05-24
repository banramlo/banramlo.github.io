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
1. A coloring of grapg $G$ is an assignment of colors to the vertices of $G$.
In this case, it can be considered as the assignemnts of sets like vertex 1 to set 1 and vetex 2 to set 3 and so-on.
2. A graph is is $k$-coloring if each vertex is assigned to exactly one of $k$ colors.
3. A coloring is proper if no two adjacent vertices are assigned to the same color.
4. A graph is so-called $k$-colorable if it has a proper $k$-coloring.
5. We say $g(n)$ $=$ $\tilde{O}(f(n))$ if there exists some constant $c$ $\ge$ $0$ such that $g(n)$ $=$ $O(f(n))\ln^c$.
This is some extention that ignoring logarithmic terms.

### Theorems
1. There is a polynomial time algorithm to check 2-colorable.
Notice that we can do it by DFS and checking there is some pair of vertices that colors collide.
2. It is known to be NP-hard to decide if a graph is 3-colorable.
3. It is known to be NP-hard to decide if a graph is 3-colorable or any of its proper coloring requires at least five colors.
This means input will give like 3-colorable, 5-colorable, 6-colorable, etc..
There will no 4-colorable inputs.
4. It is known to be NP-hard to find a 3-coloring from a 3-colorable graph.
Notice that if there is such an algorithm in polynomial time then running that algorithm and checking the result are enough to solve 3-colorable problem.



{: .box-note}
**Reference** David P. Williamson and David B. Shmoys, The Design of Approximation Algorithms.