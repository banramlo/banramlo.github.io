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
With this, we can define distance 2 neighborhood of $j$ like follow.
$N^2(j) = \\{i \in D, x_{kj}^{\star} > 0, x_{ki}^{\star} > 0\\}$.

Now, if $x_{ij}^{\star} > 0$ then $c_{ij} \le v_j^{\star}$.
Proof is like follow.

By complementary slackness, $x_{ij}^{\star} > 0$ means $v_{j}^{\star} - w_{ij}^{\star} = c_{ij}$.
Therefore, $v_j^{\star} \ge c_{ij}$.

Now, let $i_{k}^{\star}$ be the facility $i$ with the smallest $f_i$ in $N(j_{k})$ for $j_{k} \in D$.
Then $f_{i_{k}^\star}$ $=$ $f_{i_{k}^\star}\sum\limits_{i \in N(j_{k})}x_{ij_{k}}^{\star}$ $\le$ $\sum\limits_{i \in N(j_{k})}f_{i}x_{ij_{k}}^{\star}$. 
Notice that $\sum\limits_{i \in N(j_{k})}x_{ij_{k}}^{\star}$ $=$ $1$, $f_{i_{k}^\star}$ $\le$ $f_i$ for $i \in N(j_{k})$.

Then, $f_{i_{k}^\star}$ $\le$ $\sum\limits_{i \in N(j_{k})}f_{i}x_{ij_{k}}^{\star}$ $\le$ $\sum\limits_{i \in N(j_{k})}f_{i}y_{i}^{\star}$ from the constraint $x_{ij} \le y_i$.

Now, here is an approximation algorithm follow.
<div class="algorithm">
    Solve primal and dual LP and get optimum $(x^{\star}, x^{\star})$, dual optimum $(v^{\star}, w^{\star})$<br>
    $C \leftarrow D$<br>
    $k \leftarrow 0$<br>
    $\operatorname{while} C \neq \emptyset$
    <div class = "algorithm">
        $k \leftarrow k + 1$<br>
        Choose $j_k \in C$ that minimizes $v_j^{\star}$ over all $j \in C$<br>
        Choose $i_k \in N(j_k)$ to be the cheapest facility in $N(j_k)$<br>
        Open $i_k$ and assign $j_k$ and all unassigned clients in $N^2(j_k)$ to $i_k$<br>
        $C \leftarrow C - \{j_k\}  - N^2(j_k)$
    </div>
</div>

Then, the algorithm given is a 4-approximation algorithm.
Proof is like follow.

Now, let's assume that algorithm terminates in $K$ iteration.

Then, $\sum\limits_{k = 1}^{K} f_{i_k}$ $\le$ $\sum\limits_{k = 1}^{K}\sum\limits_{i \in N(j_k)} f_{i_k}y_i^{\star}$ $\le$ $\sum\limits_{i \in F} f_{i_k}y_i^{\star}$. Proof is like follow.

First inequality is what just we've showen.
For the second inequality, if we read about the algorithm, $N(j_{k_1}) \cap N(j_{k_2}) = \emptyset$ because we will assign all the $j$ in $N^2(j_k)$ at the each iteration. Therefore, claim holds.

Now, $c_{i_kl}$ $\le$ $c_{i_kj_k} + c_{hj_k} + c_{hl}$ for $l \in N^2(j_k)$ and any $h \in N(j_k) \cap N(l)$ because of the triangular inequality. 
Then, $c_{i_kl}$ $\le$ $c_{i_kj_k} + c_{hj_k} + c_{hl}$ $\le$ $v_{j_k}^{\star} + v_{j_k}^{\star} + v_l^{\star}$ because "$x_{ij}^{\star} > 0$ then $c_{ij}$ $\le$ $v_j^{\star}$".

Notice that $x_{i_kj_k} > 0$ because $i_k \in N(j_k)$,
$x_{hj_k} > 0$ because $h \in N(j_k)$,
$x_{hl} > 0$ because $h \in (N(j_k) \cap N(l)) \in N(l)$.

If we think about $v_l^{\star}$, $v_{j_k}^{\star}$ $\le$ $v_l^{\star}$ because we've choosed the minimum of $v_j^{\star}$ at each iteration.
As a result, $c_{i_kl}$ $\le$ $3v_l^{\star}$.

Therefore, $\sum\limits_{k = 1}^{K}\sum\limits_{l \in N^2(i_k)}c_{i_kl}$ $\le$ 
$\sum\limits_{k = 1}^{K}\sum\limits_{l \in N^2(i_k)}3v_l^{\star}$ $\le$
$\sum\limits_{l \in D}3v_l^{\star}$.

As a result, $\sum\limits_{k = 1}^{K} f_{i_k}$ $+$ $\sum\limits_{k = 1}^{K}\sum\limits_{l \in N^2(i_k)}c_{i_kl}$ $\le$ $\sum\limits_{i \in F} f_{i_k}y_i^{\star}$ $+$ $\sum\limits_{l \in D}3v_l^{\star}$ $\le$ $\sum\limits_{i \in F} f_{i_k}y_i^{\star}$ $+$ $\sum\limits_{i \in F, j \in D} c_{ij}x_{ij}^{\star}$ $+$ $\sum\limits_{l \in D}3v_l^{\star}$ $\le$ $\operatorname{OPT}$ + $3\operatorname{OPT}$ $=$ $4\operatorname{OPT}$.

Notice that $\sum\limits_{i \in F} f_{i_k}y_i^{\star}$ $+$ $\sum\limits_{i \in F, j \in D} c_{ij}x_{ij}^{\star}$ is an optimum of primal and
$\sum\limits_{l \in D}3v_l^{\star}$ is an optimum of dual.
Therefore both are less or equal than $\operatorname{OPT}$.

Now, we've know that the number of constraints are polynomial.
Therefore, primal and dual of LP can be solved in polynomial time.
Algorithm iterates at most $|D|$ iterations.
Therefore, it will terminates in polynomial time.
Proof stops here.


{: .box-note}
**Reference** David P. Williamson and David B. Shmoys, The Design of Approximation Algorithms.