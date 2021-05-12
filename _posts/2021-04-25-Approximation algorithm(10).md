---
layout: post
title: Approximation algorithm(10) - The uncapacitated facility location problem
gh-repo: daattali/beautiful-jekyll
tags: [algorithm, approximation]
comments: true
use_math: true
---

The uncapacitated facility location problem is a problem to decide which facility to open and how to handle clients for it.
Notice that every client will just go to the closest facility because there is no capacity limits.

Now, for a given set of facility $F$, clients $D$.
There are some costs for opening facility $f_i \ge 0$ $\forall i \in F$.
With this costs, there are some assignment costs $c_{ij} \ge 0$ $\forall i \in F, j \in D$ such that triangular inequality holds.
Which means $C_{ij}$ $\le$ $C_{il} + C_{kl} + C_{kj}$.
This means cost of "assigning $j$ to $i$" is allways less than cost of "assigning $l$ to $i$, $l$ to $k$ and $j$ to $k$".
It is not too hard constraint because $j$ can recieves item at $k$ delivered by $l$ from $i$ if it is.

Now, problem is to minimize sum of costs to assign all clients to some facility which are open.

Then, Problem can be written as IP follow.
Minimize $\sum\limits_{i \in F}f_iy_i + \sum\limits_{i \in F, j \in D} c_{ij}x_{ij}$ such that<br>
$\sum\limits_{i \in F}x_{ij} = 1$ $\forall j \in D$, <br>
$x_{ij} \le y_i$ $\forall i \in F, j \in D$ and <br>
$x_{ij}, y_i \in \\{0, 1\\}$ $\forall i \in F, j \in D$.

Notice that there is a problem similar with above but with some capacity for each facility.
In such a case, the problem is so-called "The capacitated facility location problem".

Now IP above can be LP-relaxed to below.
Minimize $\sum\limits_{i \in F}f_iy_i + \sum\limits_{i \in F, j \in D} c_{ij}x_{ij}$ such that <br>
$\sum\limits_{i \in F}x_{ij} = 1$ $\forall j \in D$, <br>
$x_{ij} \le y_i$ $\forall i \in F, j \in D$ and <br>
$x_{ij}, y_i \ge 0$ $\forall i \in F, j \in D$.

Notice that LP above is the same with LP below.

Minimize $\sum\limits_{i \in F}f_iy_i + \sum\limits_{i \in F, j \in D} c_{ij}x_{ij}$ such that <br>
$\sum\limits_{i \in F}x_{ij} = 1$ $\forall j \in D$, <br>
$y_i - x_{ij} \ge 0$ $\forall i \in F, j \in D$ and <br>
$x_{ij}, y_i \ge 0$ $\forall i \in F, j \in D$.

Now let's use $v_i, w_{ij}$ as multiplier for the first constraints and the second constraints respectively.
we can change primal LP above to dual of it.

Maximize $\sum\limits_{j \in D} v_j$ such that <br>
$w_{ij} \ge 0$ $\forall i \in F, j \in D$,<br>
$\sum\limits_{j \in D}w_{ij} \le f_i$ $\forall i \in F$,<br>
$v_j - w_{ij} \le c_{ij}$ $\forall i \in F, j \in D$.

Notice that LP above is the same with LP below.

Maximize $\sum\limits_{j \in D} v_j$ such that <br>
$w_{ij} \ge 0$ $\forall i \in F, j \in D$,<br>
$\sum\limits_{j \in D}w_{ij} \le f_i$ $\forall i \in F$,<br>
$v_j \le c_{ij} + w_{ij} $ $\forall i \in F, j \in D$.

Now, we can think it as like follow.
For each clients, they need to pay some portion of opening cost for a facility they want to go.
Therefore, let $w_{ij}$ as client $j$'s payment for the portion of opening cost.
With them, they need to pay costs for assigning them to a facility.
As a result, $v_j$ is a payment which client $j$ wii do.
Now, problem is like finding the maximum payment for clients.
However, they will pay more than they need to but less possible as.
Also, they need to pay at least the same costs for opening facility.

Now, let $(x^{\star}, y^{\star}), (v^{\star}, w^{\star})$ be an optimumm of primal and dual respectively.
Then define client $j$ is neighbor of facility $i$ if $x_{ij}^{\star} > 0$.
Therefore, let $N(j) = \\{i \in F, x_{ij}^{\star} > 0\\}$.

Now, if $x_{ij}^{\star} > 0$ then $c_{ij} \le v_j^{\star}$.
Therefore, $\sum\limits_{j \in D}v_j^{\star}$ $\ge$ $\sum\limits_{j \in D}c_{ij}$.
Proof is like follow.

By complementary slackness, $x^{ij}^{\star} > 0$ means $v_{j}^{\star} - w_{ij}^{\star} = c_{ij}$.
Therefore, $v_j^{\star} \ge c_{ij}$.

Now, let $i^{\star}$ be the facility with the smallest $c_{ij}$ in $N(j)$ for $j \in D$.
Then $f_{i^\star}$ $\le$ $f_{i^\star}\sum\limits_{i \in N(j)}x_{ij}^{\star}$ $\le$ $\sum\limits_{i \in N(j)}f_{i}x_{ij}^{\star}$. 
Notice that $\sum\limits_{i \in N(j)}x_{ij}^{\star}$ $=$ $1$, $f_{i^\star}$ $\le$ $f_i$ for $i \in N(j)$.

Then, $f_{i^\star}$ $\le$ $\sum\limits_{i \in N(j)}f_{i}x_{ij}^{\star}$ $\le$ $\sum\limits_{i \in N(j)}f_{i}y_{i}^{\star}$ from the constraint $x_{ij} \le y_i$.


{: .box-note}
**Reference** David P. Williamson and David B. Shmoys, The Design of Approximation Algorithms.