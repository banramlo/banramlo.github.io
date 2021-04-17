---
layout: post
title: Network decomposition
gh-repo: daattali/beautiful-jekyll
tags: [algorithm]
comments: true
use_math: true
---

A graph is a typical data structure which is used to represent some relations between nodes.
From any graph, we can imagine a flow of a graph $G$.
Formally, flow of the graph is defined like follow.
For a given graph $G= (V, E)$, flow $f$ is a mapping function $V \times V \rightarrow \mathcal{R}$.
To be a flow, there are some requirements.
If there is a capacity $c$ for edeges $c : V \times V \rightarrow \mathcal{R}$, which $(u,v) \in E$ iff $c(u,v)$ exists.
Then flow of graph can't exceed that capacity.
Which means $f(u,v) \le c(u, v)$.
Also it requires two variables which are source $s$ and destination $t$.
Then $\sum\limits_{(i, v) \in E, v \neq s,t}f(i, v) = \sum\limits_{(v, j) \in E, v \neq s,t}f(v, j)$.
Which means that every value goes in $v$ should go out from $v$ either.
Then we can evaluate this flow $f$ as $\sum\limits_{(s, v) \in E}f(s, v) - \sum\limits_{(v, s) \in E}f(v, s)$ $=$ $\sum\limits_{(v, t) \in E}f(v, t) - \sum\limits_{(t, v) \in E}f(t, v)$.

Now, let's think about a directed graph $G = (V,E)$ and some flow $f$ of $G$.
Then we can decompose $f$ to some simple paths $P_1, \cdots, P_N$ and some cycles $C_1, \cdots, C_M$.
Which $N + M \le |E|$.

Without loosing much of generality, we can assume that $f$'s value is positive or negative.
In the proof of flow decomposition, it doesn't necessarily to be positive or negative.
However, let's assume it to be positive for easy.

Then, if $f$ has a cycle $C$, we can eliminate that $C$ from $f$ without changing value of the flow because it is a cycle.
If we reduce every value of edges with smallest value inside of the $C$ then we can remove that cycle.
Notice that we can reduce value of edges because we picked the smallest value inside of the $C$,
cycle should be disappear after reducing values because one of edge in $C$ should be 0 after reducing values.

If we remove every cycle, it should have some paths because it has a flow.
If we remove any flow that has the smallest value, value of flow $f$ should be 0 or some positive value.
If it is the first case (flow of $f$ become 0) then it is decomposed.
Otherwise, we can do this process again.
Therefore, there sould be at least some cycles and paths.

Notice that we should remove at least 1 edge to remove a path or a cycle because it choosed a smallest value on graph.
As a result, we can do this at most $|E|$.

{: .box-note}
**Reference** David P. Williamson and David B. Shmoys, The Design of Approximation Algorithms.