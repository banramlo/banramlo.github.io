---
layout: post
title: Approximation algorithm(11) - survivable network design and iterated rounding
gh-repo: daattali/beautiful-jekyll
tags: [algorithm, approximation]
comments: true
use_math: true
---

Survivable network design is a problem that designing a network that can survive from disconnecting some edges.

Let's assume there is a given undirected graph $G = (V,E)$ with costs $c_e \ge 0$ $\forall e \in E$.
Now, let's define connectivity requirments $r_{ij} \in \mathbb{Z}^{+} \cup \\{0\\}$ for all pairs of vertices $i,j \in V$, where $i \neq j$.
Then problem is a find a minimum cost set of edges $F \subset E$ such that $G' = (V,F)$ has $r_{ij}$ edge distinct paths to connecting $i$ and $j$.

Now, we can do a integer programming relaxation over this and it is like follow.
"Minimize $\sum\limits_{e \in E} c_e x_e$ such that $\sum\limits_{e \in \delta(S)} x_e \ge \max\limits_{i \in S, j \not\in S} r_{ij}$ $\forall S \subset V$, $x_e \in \\{0,1\\}$" which $\delta(S)$ denotes the set of edges between $S$ and $V - S$.
Notice that if there is $r_{ij}$ edge distinct paths, we can make flow of value $r_{ij}$ between $i$ and $j$.
Then, there should be a mincut that corresponding to that flow from the network flow theory.
As a result, constraint above should fullfilled.
It's the same in the opposite direction.
If we have a such min-cut then we have such flow either.

Now, we can do a linear programming relaxation like we did in the set cover.
"Minimize $\sum\limits_{e \in E} c_e x_e$ such that $\sum\limits_{e \in \delta(S)} x_e \ge \max\limits_{i \in S, j \not\in S} r_{ij}$ $\forall S \subset V$, $0 \le x_e \le 1$".
We can solve this LP in polynomial time with ellipsoid method.
We can construct seperation orcale like follow.
1. Make a graph that consists of $G = (V,E)$ and set capacity of $E$ as $x_e$.
2. Solve the network flow problem between all pair $i$ and $j$ such that $i \neq j$ and $i,j \in V$.
3. If there is a flow such that the value of flow $f_{ij}$ is less than $r_{ij}$ then it's an infeasible solution otherwise it is feasible.

Now, let's define $f(S) = \max\limits_{i \in S, j \not\in S} r_{ij}$.
Then problem will be shifted to "Minimize $\sum\limits_{e \in E} c_e x_e$ such that $\sum\limits_{e \in \delta(S)} x_e \ge f(S)$ $\forall S \subset V$, $0 \le x_e \le 1$".

If we make $f(S)$ as so then $f(S)$ is so-called weakly supermodular.
To be a weakly supermodular, there are some requirements.
1. $f(\emptyset) = f(U) = 0$ for ground set $U$.
2. one of $f(A) + f(B) \le f(A \cap B) + f(A \cup B)$ or $f(A) + f(B) \le f(A - B) + f(B - A)$ should be true.

Proof is like follow.
First, it is trivial that $f(\emptyset) = f(V) = 0$ because there are no such an edge between $V$ and $\emptyset$.
Before we prove the second requirments, $f(A) = f(V - A)$ is trivial either.
Also, we have $max(f(A), f(B)) \ge f(A \cup B)$.
Because if we think about $i \in A \cup B, j \not\in A \cup B$ then it should be one of "$i \in A, j \not\in A$ or $i \in B, j \not\in B$".

Now, we can make 4 property of $f$ from the fact above.

1. $f(A) \le \max(f(A - B), f(A \cap B))$
2. $f(A) = f(V - A) \le \max(f(B - A), f(V - (A \cup B))) = \max(f(B - A), f(A \cup B))$
3. $f(B) \le \max(f(B - A), f(A \cap B))$
4. $f(B) = f(V - B) \le \max(f(A - B), f(V - (A \cup B))) = \max(f(A - B), f(A \cup B))$

Notice that $(A - B) \cup (A \cap B) = A$, $(V - (A \cup B)) \cup (B - A) = V - A$, $(B - A) \cup (A \cap B) = B$ and $(V - (A \cup B)) \cup (A - B) = V - B$.
This implies 4 inequalities.
1. $f(A) + f(B) \le \max(f(A - B), f(A \cap B)) + \max(f(B - A), f(A \cap B))$
2. $f(A) + f(B) \le \max(f(A - B), f(A \cap B)) + \max(f(A - B), f(A \cup B))$
3. $f(A) + f(B) \le \max(f(B - A), f(A \cup B)) + \max(f(B - A), f(A \cap B))$
4. $f(A) + f(B) \le \max(f(B - A), f(A \cup B)) + \max(f(A - B), f(A \cup B))$

Then, we can make possible inequality for weakly supermodular in any case.
1. $f(A) + f(B) \le \max(f(A - B), f(A \cap B)) + \max(f(B - A), f(A \cap B)) = f(A - B) + f(B - A)$ if $f(A - B), f(B - A) \ge f(A \cap B)$
2. $f(A) + f(B) \le \max(f(A - B), f(A \cap B)) + \max(f(A - B), f(A \cup B)) = f(A \cap B) + f(B \cup A)$ if $f(A \cap B), f(B \cup A) \ge f(A - B)$
3. $f(A) + f(B) \le \max(f(B - A), f(A \cup B)) + \max(f(B - A), f(A \cap B)) = f(A \cap B) + f(B \cup A)$ if $f(A \cap B), f(B \cup A) \ge f(B - A)$
4. $f(A) + f(B) \le \max(f(B - A), f(A \cup B)) + \max(f(A - B), f(A \cup B)) = f(A - B) + f(B - A)$ if $f(A - B), f(B - A) \ge f(A \cup B)$
Notice that if $f(A \cap B), f(B \cup A) \ge f(A - B)$, $f(A \cap B), f(B \cup A) \ge f(B - A)$ doesn't right then at least one of $f(A - B), f(B - A) \ge f(A \cap B)$ or $f(A - B), f(B - A) \ge f(A \cup B)$ should be true.
As a result, $f$ is a weakly supermodular

Now, if we solve linear problem "Minimize $\sum\limits_{e \in E} c_e x_e$ such that $\sum\limits_{e \in \delta(S)} x_e \ge f(S)$ $\forall S \subset V$, $0 \le x_e \le 1$",
there exists an edge $e \in E$ such that $x_e \ge \frac{1}{2}$ for any weakly supermodular $f$.

From the fact above, we can construct an algorithm.

<div class="algorithm">
    $F \leftarrow \emptyset$<br>
    $i \leftarrow 1$<br>
    $\operatorname{while}$ $F$ is not a feasible solution $\textbf{do}$<br>
    <div class = "algorithm">
        Solve "Minimize $\sum\limits_{e \in E - F} c_e x_e$ such that $\sum\limits_{e \in \delta(S), e \in E - F} x_e \ge f_i(S) = f(S) - |\delta(S) \cap F|$ $\forall S \subset V$, $0 \le x_e \le 1$"<br>
        $F_i \leftarrow \left{ e \in E - F : x_e \ge \frac{1}{2} \right}$<br>
        $F \leftarrow F \cup F_i$<br>
        $i \leftarrow i + 1$
    </div>
    $\textbf{return} \text{ } F$
</div>


{: .box-note}
**Reference** David P. Williamson and David B. Shmoys, The Design of Approximation Algorithms.