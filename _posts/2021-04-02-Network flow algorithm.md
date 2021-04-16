---
layout: post
title: Network decomposition
gh-repo: daattali/beautiful-jekyll
tags: [algorithm]
comments: true
use_math: true
---

Graph is a typical data structure which is used to represent some relations between nodes.
From any graph we can imagine a flow of a graph $G$.
Formally, flow of the graph is defined like follow.
For a given graph $G= (V, E)$, flow $f$ is a mapping function $V \times V \rightarrow \mathcal{R}$.
To be a flow, there are some requirements.
If there is a capacity $c : V \times V \rightarrow matcal{R}$, which $(u,v) \in E$ if $c(u,v)$ exists.
Then, flow of graph can't exceed that capacity which means $f(u,v) \le c(u, v)$.
Also, it requires two variable which are source $s$ and destination $t$.
Then, $\sum\limits_{(i, v) \in E, v \not\eq s,t}f(i, v) = \sum\limits_{(v, j) \in E, v \not\eq s,t}f(v, j)$.
Which means that every value goes in $v$ then every value should go out from $v$ either.
Then we can evaluate this flow $f$ as $\sum\limits_{(s, v) \in E}f(s, v) - \sum\limits_{(v, s) \in E}f(v, s)$ $=$ $\sum\limits_{(v, t) \in E}f(v, t) - \sum\limits_{(t, v) \in E}f(t, v)$.

Now, let's think about a directed graph $G = (V,E)$ and some flow $f$ of $G$.
Then we can decompose $f$ to some simple paths $P_1, \cdots, P_N$ and some cycles $C_1, \cdots, C_M$.
Which $N + M \le |E|$.
Without loosing much of generality, we can assume that $f$'s value is positive or negative.
In the proof of this flow decomposition, it doesn't necessarily to be positive or negative.
However, let's assume it to be positive for easy.

Then, if $f$ has a cycle $C$, we can eliminate that $C$ with smallest value in side of the $C$ from $f$ without changing value of the flow because it is a cycle.
If we remove every cycle, it should have some paths because it has a flow.
If we remove any flow that has smallest value, then value of flow $f$ should be 0 or some value.
If it is the first case (flow of $f$ become 0) then it is decomposed.
Otherwise(there is some value) then we can do this process again.
Therefore, there sould be at least some cycles and paths.

Notice that we should remove at least 1 edge to remove a path or a cycle.
If it didn't removing path cna't be happen because it choosed a smallest value on graph.
It's the same case for cycle because it should remove a edge to resolve the cycle.

{: .box-note}
**Reference** David P. Williamson and David B. Shmoys, The Design of Approximation Algorithms.