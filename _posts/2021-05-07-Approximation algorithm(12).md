---
layout: post
title: Approximation algorithm(12) - The uncapacitated facility location problem(2)
gh-repo: daattali/beautiful-jekyll
tags: [algorithm, approximation]
comments: true
use_math: true
---

Now, we will re visit the uncapacitated facility location problem.
Before we proceed new methodology for the uncapacitated facility location problem, we can extend some terminology for assignment costs between clients or facilities either.
For example, assignment costs between client $i$ and $j$ can be the shortest path between client $i$ and $j$.
If we define this as so, it is known to reserve the solution because of the metric closure.

Now, let's recap the problem itself.

Minimize $\sum\limits_{i \in F}f_iy_i + \sum\limits_{i \in F, j \in D} c_{ij}x_{ij}$ such that<br>
$\sum\limits_{i \in F}x_{ij} = 1$ $\forall j \in D$, <br>
$x_{ij} \le y_i$ $\forall i \in F, j \in D$ and <br>
$x_{ij}, y_i \in \\{0, 1\\}$ $\forall i \in F, j \in D$.

Then, we can do LP-relaxation below.

Minimize $\sum\limits_{i \in F}f_iy_i + \sum\limits_{i \in F, j \in D} c_{ij}x_{ij}$ such that <br>
$\sum\limits_{i \in F}x_{ij} = 1$ $\forall j \in D$, <br>
$x_{ij} \le y_i$ $\forall i \in F, j \in D$ and <br>
$x_{ij}, y_i \ge 0$ $\forall i \in F, j \in D$.

With above, LP dual is like follow.

Maximize $\sum\limits_{j \in D} v_j$ such that <br>
$w_{ij} \ge 0$ $\forall i \in F, j \in D$,<br>
$\sum\limits_{j \in D}w_{ij} \le f_i$ $\forall i \in F$,<br>
$v_j - w_{ij} \le c_{ij}$ $\forall i \in F, j \in D$.

Now let's define some terminologies for a given dual solution $v^{\star}.

1. Client $j$ is neighbor of facility $i$ if $v^{\star}_j$ $\ge$ $c_{ij}$, w^{\star}$.
With that let denotes $N(i)$ as the set of neighbors of $i$.
2. Client $i$ contributes to $i$ if $w^{\star}_{ij} > 0$.

Now, let's desing the algorithm.

<div class="alg">
    $v \leftarrow 0, w \leftarrow 0$<br>
    $S \leftarrow D$ // Left clients to be allocated<br>
    $T \leftarrow \emptyset$ // Facility which are open<br>
    $operatorname{while}$ $S \neq \emptyset$<br>
    <div class="alg">
        Increase $v_j$ for all $j \in S$ and $w_{ij}$ for all $i$ $\in$ $N(j)$, $j$ $\in$ $S$ uniformly until some $j$ $\in$ $S$ neighbors some $i$ $\in$ $T$ or some $i$ $\not\in$ $T$ has a tight dual inequality<br>
        $\operatorname{if}$ some $j$ $\in$ $S$ neighbors some $i$ $\in$ $T$<br>
        <div class="alg">
            $S \leftarrow S - \{j\}$
        </div>
        $\operatorname{if}$ some $i$ $\not\in$ $T$ has a tight dual inequality<br>
        <div class="alg">
            $T \leftarrow T \cup \{i\}$<br>
            $S \leftarrow S - N(i)$
        </div>
    </div>
    $T' \leftarrow \emptyset$<br>
    $\operatorname{while}$ $T$ $\neq$ $\emptyset$<br>
    <div class="alg">
        $T' \leftarrow T' \cup \{i\}$<br>
        $T \leftarrow T - \{h \in T : \exists j \in D, w_{ij} > 0 \text{ and } w_{hj} > 0\}$<br>
    </div>
</div>


{: .box-note}
**Reference** David P. Williamson and David B. Shmoys, The Design of Approximation Algorithms.