---
layout: post
title: Parallel graph algorithm(1)
gh-repo: daattali/beautiful-jekyll
tags: [algorithm, concurrency, graph]
comments: true
use_math: true
---

## Matrix representation
There are many formats to represent matrices.

| Format            | Adjacency matrix  | COO(Edge list(Array))   | Edge list(linked list)  | Adjacency list   | CSR/CSC          |
| :------           | :------           | :------                 | :------                 | :------          | :------          |
| Space             | $O (\right\vert V \left\vert^2)$       | $O(\right\vert E \left\vert)$                | $O(\right\vert E \left\vert)$                | $O(\right\vert V \left\vert + \right\vert E \left\vert)$   | $O(\right\vert V \left\vert + \right\vert E \left\vert)$   |
| Read edge         | $O (1)$           | $O(log \right\vert E \left\vert)$            | $O(\right\vert E \left\vert)$                | $O(deg(v))$      | $O(deg(v))$      |
| Add edge          | $O (1)$           | $O(\right\vert E \left\vert)$                | $O(\right\vert E \left\vert)$                | $O(deg(v))$      | $O(\right\vert V \left\vert + \right\vert E \left\vert)$   |
| Delete edge       | $O (1)$           | $O(\right\vert E \left\vert)$                | $O(\right\vert E \left\vert)$                | $O(deg(v))$      | $O(\right\vert V \left\vert + \right\vert E \left\vert)$   |
| Read neighbors    | $O (\right\vert V \left\vert)$         | $O(log \right\vert E \left\vert + deg(v))$   | $O(\right\vert E \left\vert)$                | $O(deg(v))$      | $O(deg(v))$      |
| Get degree        | $O (\right\vert V \left\vert)$         | $O(log \right\vert E \left\vert + deg(v))$   | $O(\right\vert E \left\vert)$                | $O(deg(v))$      | $O(1)$           | 

Adjacency matrix is a matrix representation as it is.
Adjacency matrix uses 2D-array to store every value.
Edge list is a matrix representation which represents edges to an array(or a linked list).
Notice that we can find a specific edge by binary search.
Adjacency list is a matrix representation which repreents edges as a list per vertex and keep array of vertex as an array.
CSR/CSC is a matrix representation which repreents matrix like adjacency matrix but using an array.
CSC/CSC uses a row_ptr/col_ptr to noticing the starting and ending index of each edges of a row/column.
With these indices, it represents the column/row indices and values.

Each format has own pros and cons.
However, there are some reasons which makes some formats aren't be used in the mordern science. 
In real world matrices, may of them are sprase.
Therefore, adjacency matrix isn't used anymore.
Edge list and Adjacency list(which uses linked list) aren't used because of dynamic memory hierarchy.

COO representation are rarely used but CSR/CSC are more standards because of memory usage is slightly different.
CSC/CSC uses $\right\vert V \left\vert + 2\right\vert E \left\vert$ memories but COO uses $3\right\vert E \left\vert$ memories.