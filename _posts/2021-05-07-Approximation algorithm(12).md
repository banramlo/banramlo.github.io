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

Now let's define some terminologies for a given dual solution $v^{\star}, w^{\star}$.

1. Client $j$ is neighbor of facility $i$ if $v_{j}^{\star}$ $\ge$ $c_{ij}$.
With that let denotes $N(i)$ as the set of neighbors of $i$.
2. Client $j$ contributes to $i$ if $w_{ij}^{\star} > 0$.

Now, let's desing the algorithm.

<div class="alg">
    $v \leftarrow 0, w \leftarrow 0$<br>
    $S \leftarrow D$ // Left clients to be allocated<br>
    $T \leftarrow \emptyset$ // Facility which are open<br>
    $\operatorname{while}$ $S$ $\neq$ $\emptyset$<br>
    <div class="alg">
        Increase $v_j$ for all $j \in S$ and $w_{ij}$ for all $i$ $\in$ $N(j)$, $j$ $\in$ $S$ uniformly until some $j$ $\in$ $S$ neighbors some $i$ $\in$ $T$ or some $i$ $\not\in$ $T$ has a tight dual inequality<br>
        $\operatorname{if}$ some $j$ $\in$ $S$ neighbors some $i$ $\in$ $T$ // $j$ paid enough to assigned to facility $i$<br>
        <div class="alg">
            $S \leftarrow S - \{j\}$
        </div>
        $\operatorname{if}$ some $i$ $\not\in$ $T$ has a tight dual inequality // $i$'s funding collected enough to be opened<br>
        <div class="alg">
            $T \leftarrow T \cup \{i\}$<br>
            $S \leftarrow S - N(i)$
        </div>
    </div>
    $T' \leftarrow \emptyset$ // $T'$ is the set of facility to be<br>
    $\operatorname{while}$ $T$ $\neq$ $\emptyset$<br>
    <div class="alg">
        Pick $i$ $\in$ $T$<br>
        $T' \leftarrow T' \cup \{i\}$<br>
        $T \leftarrow T - \{h \in T : \exists j \in D, w_{ij} > 0 \text{ and } w_{hj} > 0\}$<br>
    </div>
</div>

Now, if $j$ doesn't have a neighbor in $T'$ then there exists some $i \in T'$ such that $c_{ij} \le 3v_j$ if we work with algorithm above.
Let's think about $T$ at the end of "$\operatorname{while}$ $S$ $\neq$ $\emptyset$" and $T'$ at the end of the algorithm.
Then, all $j$'s neighbors will be excluded in $T$ at the loop of "$\operatorname{while}$ $T$ $\neq$ $\emptyset$" to be not exists in $T'$.
Let denotes $h$ as the neighbor of $j$ which $v_j$ stops to increase.
Notice that $h$ is not in $T'$ and $h$ should be a neighbor of $j$.
Then, there exists some $k$ that contributes $h$ and $i$ $\in$ $T'$ for all $h$.
First of all, if $j$ contributes to $i$ then $j$ is neighbor of $i$.
If we think about the algorithm itself, we increase $w_{ij}$ only if $i$ $\in$ $N(j)$.
As a result, $k$ need to be a neighbor of both $h$ and $i$.
At the same time $j$ is a neighbor of $h$. 
Now, $c_{ij}$ $\le$ $c_{ik}$ $+$ $c_{hk}$ $+$ $c_{hj}$ from the triangle inequality.
Then, $c_{ik}$ $+$ $c_{hk}$ $+$ $c_{hj}$ $\le$ $v_{k}$ $+$ $v_{k}$ $+$ $v_{j}$ $=$ because each of them are in the neighbor relation.

With above, $v_k$ $\le$ $v_j$.
Proof is like follow.
Let's assume not.
$v_k, w_{hk}$ will stop increasing at the moment of between "$k$ neighbors $h$ $\in$ $T$" or "$h$ $\not\in$ $T$ become tight and $k$ neighbors $h$".
However we should select second case because $k$ can increase $w_{hk}$ only after $k$ neighbors $h$.
At the same time, $v_j$ will stop increasing at the moment of between "$j$ neighbors $h$ $\in$ $T$" or "$j$ $\not\in$ $T$ become tight and $k$ neighbors $h$".
In the first case, $h$ $\in$ $T$ already but $k$ stoped increasing $v_k$ when $h$ become tight therefore $v_k$ $<$ $v_j$.
In the second case, both $v_j$ and $v_k$ stop increasing at the same time. As a result $v_k$ $=$ $v_j$.
Therefore claim holds.

Now, we can prove that this algorithm is a 3-approximation algorithm.
Even though algorithm increase uniformly, we can just pick the smallest delta value of constraints to run this algorithm in polynomial time.
With this, all other parts of algorithm is polynomial therefore, this algorithm runs in polynomial time.

Now, let's think about the solution that assigning $i$ to $j$ which $i$ that contributes to $j$ and assigning $i$ to arbitrary facility in $T'$ for $i$ that doesn't contributes to any of facility.
Notice that from the algorithm we forced it to contributes to only one facility or none of facility.
If we define $A(i)$ as the set of assigned clients to faciltiy $i$, total cost will be $\sum\limits_{i \in T'}(f_i + \sum\limits_{j \in A(i)}c_{ij})$.
Then, $\sum\limits_{i \in T'}(f_i + \sum\limits_{j \in A(i)}c_{ij})$ $=$ $\sum\limits_{i \in T'}\sum\limits_{j \in A(i)}(w_{ij} + c_{ij})$ $=$ $\sum\limits_{i \in T'}\sum\limits_{j \in A(i)}v_{j}$.
Notice that first equality comes from that $i$ $\in$ $T'$ $\subseteq$ $T$ and $T$ is the set of facility that become tight.
At the same time, $w_{ij}$ $+$ $c_{ij}$ $=$ $v_j$ because it should be tight.


{: .box-note}
**Reference** David P. Williamson and David B. Shmoys, The Design of Approximation Algorithms.