---
layout: post
title: Approximation algorithm(10) - The uncapacitated facility location problem
gh-repo: daattali/beautiful-jekyll
tags: [algorithm, approximation]
comments: true
use_math: true
---

The uncapacitated facility location problem is a problem to decide which facility to open and how to handle to clients for it.
Notice that every client will just go to the closest facility because there is no capacity limits.

Now, for a given set of facility $F$, clients $D$.
There are some costs for opening facility $f_i \ge 0$ $\forall i \in F$.
With this costs, there are some assignment costs $c_{ij} \ge 0$ $\forall i \in F, j \in D$ such that triangular inequality holds.
Which means $C_{ij}$ $\le$ $C_{il} + C_{kl} + C_{kj}$.
This means cost of "assigning $j$ to $i$" is allways less than cost of "assigning $l$ to $i$, $l$ to $k$ and $j$ to $k$".
It is not too hard constraint because $j$ can recieves item at $k$ delivered by $l$ from $i$ if it is.

Now, problem is to minimize sum of costs to assign all clients to some facility which are open.

Then, Problem can be IP follow.
Minimize $\sum\limits_{i \in F}f_iy_i + \sum\limits_{i \in F, j \in D} c_{ij}x_{ij}$ 
such that
$\sum\limits_{i \in F}x_{ij} = 1$ $\forall j \in D$, 
$x_{ij} \le y_i$ $\forall i \in F, j \in D$ and 
$x_{ij}, y_i \in \\{0, 1\\}$ $\forall i \in F, j \in D$.

Notice that there is a problem similar with above but with some capacity for each facility.
In such a case, the problem is so-called "The capacitated facility location problem".

Now IP above can be LP-relaxed to below.
Minimize $\sum\limits_{i \in F}f_iy_i + \sum\limits_{i \in F, j \in D} c_{ij}x_{ij}$ 
such that $\sum\limits_{i \in F}x_{ij} = 1$ $\forall j \in D$, 
$x_{ij} \le y_i$ $\forall i \in F, j \in D$ and 
$x_{ij}, y_i \ge 0$ $\forall i \in F, j \in D$.

Now let's use $v_i$ to multiplier for the first constraints and $w_{ij}$ for the second constraints.
we can change primal LP above to dual of it.

Maximize $\sum\limits_{j \in D} v_j + \sum\limits_{i \in F, j \in D}w_{ij}$
such that $w_{ij} \le 0$ $\forall i \in F, j \in D$,
$\sum\limits_{i \in F}c_{ij}v_j \le 0$ $\forall j \in D$,
$c_{ij}w_{ij} \le 0$ $\forall i \in F, j \in D$.

{: .box-note}
**Reference** David P. Williamson and David B. Shmoys, The Design of Approximation Algorithms.