---
layout: post
title: Approximation algorithm(11) - Generalized Steiner tree problem
gh-repo: daattali/beautiful-jekyll
tags: [algorithm, approximation]
comments: true
use_math: true
---

There is a problem known as a steiner tree problem.
The steiner tree problem is a problem to find a minimum edge set that connects every vertex in $S \subseteq V$ from a given graph $G = (V,E)$.
Notice that this problem doesn't care whether you included some vertices $v \in V - S$ in the edge set.
Moreover, this problem is a superset of the minimum spanning tree problem and shortest path problem.
We can construct problem like $S = V$ or $S = \\{s, t\\}$ to solve the minimum spanning tree problem and shortest path problem repectively.
Notice that this problem will result in a tree because it will not have a cycle which doesn't make sense for minimum edge set with connectivity.

Generalized steiner tree problem is a problem that requires to connect some pairs of vertices of given graph $G$.
If we select one vertex $v$ and set a required pairs as $v$ and others, it's the steiner tree problem.
Which means Generalized steiner tree problem is a generalized version of steiner tree problem like it named.
At the same time, it's a special case of survivable network design problem which all $r_{ij} = 1$.
Now let's define $\delta(S) = \\{e | e = (u,v) \text{ such that } u \in S, v \in V - S \\}$ and $S_i = \\{S \subseteq V : \left\vert S \cap \\{s_i, t_i\\} \right\vert = 1\\}$.
Notice that $S_i$ is a set of subsets that make a proper s-t cut for $s_i, t_i$.
Therefore, problem can be written as LP follow.

For given pairs of vetices $s_i, t_i$,<br>
minimize $\sum\limits_{e \in E}c_e x_e$ such that<br>
$\sum\limits_{e \in \delta(S)} x_e \ge 1$, for all $S \subseteq V$ which $S \in S_i$ for some $i$, <br>
$x_e \in \\{0, 1\\}$.

Now IP above can be relaxed to LP below.

For given pairs of vetices $s_i, t_i$,<br>
minimize $\sum\limits_{e \in E}c_e x_e$ such that<br>
$\sum\limits_{e \in \delta(S)} x_e \ge 1$, for all $S \subseteq V$ which $S \in S_i$ for some $i$, <br>
$x_e \ge 0$.

Now let's make a dual of primal like follow.

Maximize $\sum\limits_{e \in \delta(S), S \in S_i, \forall i}y_S$ such that<br>
$\sum\limits_{e \in \delta(S)} y_S \le c_e$ for all $e \in E$<br>
$y_S \ge 0$.

Now, let's think it as following for intuition.
$v \in V$ will be spread at the space with edge distance $c_e$.
For any $S \subseteq V$, we can think about some moats surrounding that $S$.
Now, let's think a width of moat for $S$ as $y_S$.
Then it can be thought as maximizing moat's width.
With constraint that moats can't overlapped.

Then we can try algorithm follow.

<div class="alg">
    $y \leftarrow 0$<br>
    $F \leftarrow \emptyset$<br>
    $\operatorname{while}$ not all $s_i-t_i$ pairs are connected in $(V,F)$<br>
    <div class="alg">
        Let $C$ be a connected component of $(V,F)$ such that $\left\vert C \cap \{s_i, t_i\} \right\vert = 1$ for some $i$<br>
        Increase $y_C$ until there is an edge $e \in \delta(C)$ such that $c_{e}$ $=$ $\sum\limits_{S:e \in \delta(S)}y_S$<br>
        $F \leftarrow F \cup \{e\}$<br>
    </div>
    return $F$
</div>

Algorithm above works like choose any componenet such that are not tight then increase moat distance as possible as.

Then, LP optimum is $\sum\limits_{e \in F}c_e$ because of strong duality.
However, $\sum\limits_{e \in F}c_e$ $=$ $\sum\limits_{e \in F}\sum\limits_{S:e \in \delta(S)} y_S$ $=$ $\sum\limits_{S}\sum\limits_{e \in F \cap \delta(S)} y_S$ $=$ $\sum\limits_{S}|F \cap \delta(S)|y_S$.

However, algorithm above can't guarantees to be nice approximation algorithm.
Let's think about the complete graph $G = (\\{s,t_1,t_2,t_3,\cdots,t_n\\},E)$ with $C_{(s,t_1)} = C_{(s,t_2)} = \cdots = C_{(s,t_n)} > 0$.
If pairs set to be connected is $(s,t_1), (s,t_2), \cdots, (s,t_n)$ then $\sum\limits_{S}|F \cap \delta(S)|y_S$ = $\sum\limits_{S}ny_S$.
Which means this is $O(n)OPT$ approximation.
However it's not good enough even though the solution we will get is an good solution.
Therefore, we will modify algorithm above bit to below.

<div class="alg">
    $y \leftarrow 0$<br>
    $l \leftarrow 0$<br>
    $F \leftarrow \emptyset$<br>
    $\operatorname{while}$ not all $s_i-t_i$ pairs are connected in $(V,F)$<br>
    <div class="alg">
        $l \leftarrow l + 1$<br>
        Let $\mathcal{C}$ be the set of all connected components $C$ of $(V,F)$ such that $\left\vert C \cap \{s_i, t_i\} \right\vert = 1$ for some $i$<br>
        Increase $y_C$ for all $C \in \mathcal{C}$ uniformly until for some $e_l \in \delta(C)$, $C \in \mathcal{C}$, $c_{e_l}$ $=$ $\sum\limits_{S:e_l \in \delta(S)}y_s$<br>
        $F \leftarrow F \cup \{e_l\}$<br>
    </div>
    $F' \leftarrow F$<br>
    $\operatorname{for} k \leftarrow l,l-1,\cdots,1$
    <div class="alg">
        $\operatorname{if}$ $F' - e_k$ is a feasible solution then
        <div class="alg">
            Remove $e_k$ from $F'$
        </div>
    </div>
    return $F'$
</div>

Notice that algorithm is a polynomial algorithm.
Finding connected components is polynomial algorithm by BFS.
Increasing $y_C$ is polynomial algorithm because we can pick some $\Delta y_C$ to be tight.
Then it is enough to pick minimum of such an $\Delta y_C$ for uniform increasement.

First of all, $\sum\limits_{C \in \mathcal{C}} \left\vert \delta(C) \cap F' \right\vert$ $\le$ $2 \left\vert \mathcal{C} \right\vert$.
Proof is like follow.

Let $F_i = \\{e_1,e_2,\cdots,e_{i-1}\\}$, $H_i = F' - F_i$.
Notice that $F_i \cup H_i = F_i \cup F'$ is a feasible soltuion.
Also, if we remove any edge from $H_i$ then $F_i \cup H_i$ will be a non-feasible soluiton.
Notice that we only removed edges that doesn't change feasibility on the progress.

With above, $F_i$ is a foreset for all $i$ because algorithm will increase only $y_C$ such that $C \in \mathcal{C}$.
However, two end points of $y_C$ need to in the seperate componenets to be that. 
Because $\mathcal{C}$ is the set of all connected components $C$ of $(V,F)$ such that $\left\vert C \cap \{s_i, t_i\} \right\vert = 1$ for some $i$ that are not connected.
As a result, $F_i$ is a forest.

Now let's think about a super graph $G_i$ that changes every connected componenets of $(V, F_i)$ to a single vertex.
Then $G_i$ is a forest because $G$ was a forest.
With that, no edge in $H_i$ can't have both end points in one connected component because it will make a cycle.
Therefore, $G_i$ will not have a loop either.
Now, let $V_i$ as the vertex set of $G_i$ and degree of that vertex to $\operatorname{deg}(v)$ for $v \in V_i$.

With that, let $v \in V_i$ is active if $\left\vert C \cap \\{S_j, t_j\\}\right\vert$ $=$ $1$ for some $j$.
Then, let $A_i$ as the set of active components in $V_i$.

Now, let's go back to the "$\sum\limits_{C \in \mathcal{C}} \left\vert \delta(C) \cap F' \right\vert$ $\le$ $2 \left\vert \mathcal{C} \right\vert$".
Then, $\mathcal{C}$ should be active because we pick "$\mathcal{C}$ be the set of all connected components $C$ of $(V,F)$ such that $\left\vert C \cap \{s_i, t_i\} \right\vert = 1$ for some $i$".
Therefore, RHS is $2 \left\vert A_i \right\vert$.
Also, no edge in $F_i$ can be in $\delta(C)$ for $C \in \mathcal{C}$ because $F' \subseteq F_i \cup H_i$.
Notice that even we used $\mathcal{C}$ but it's $\mathcal{C}$ at the $i$ th iteration and $C$ is a connected componenet of $F_i$.
Therefore, $\sum\limits_{C \in \mathcal{C}} \left\vert \delta(C) \cap F' \right\vert$ = $\sum\limits_{C \in \mathcal{C}} \left\vert \delta(C) \cap H_i' \right\vert$ $=$ $\sum\limits_{v \in A_i} deg(v)$ because there is no edge in side of ecah connected component to be grow. 

Therefore, showing $\sum\limits_{v \in A_i} deg(v)$ $\le$ $2 \left\vert A_i \right\vert$ is enough.
Proof is like follow.

First, no $v \in V_i - A_i$ has degree one.
If there is any $v \in V_i - A_i$ has degree one, let $e = (v, u)$.
However, it means $e$ was not removed which means $e$ was necessary for feasiblity.
Which means $|C_{V_i} \cap \\{S_j, t_j\\}| = 1$ for some $j$.
However, this is contradict with $v \not\in A_i$.

Let $B_i$ $=$ $\\{v \in V_i - A_i \vert deg(v) > 0\\}$.
Then, $\sum\limits_{v \in A_i} deg(v)$ $=$ $\sum\limits_{v \in A_i \cup B_i} deg(v)$ $-$ $\sum\limits_{v \in B_i} deg(v)$.
Notice that $A_i \cap B_i = \emptyset$.
Then $\sum\limits_{v \in A_i \cup B_i} deg(v)$ $-$ $\sum\limits_{v \in B_i} deg(v)$ $\le$ $2(\left\vert A \right\vert + \left\vert B \right\vert)$ $-$ $2\left\vert B \right\vert$.
Notice that this graph is a forest and therefore sum of degree can't bigger than twice of size of the set because there are less edge than number of vertices.
$\sum\limits_{v \in A_i} deg(v)$ $=$ $\sum\limits_{v \in A_i \cup B_i} deg(v)$ $-$ $\sum\limits_{v \in B_i} deg(v)$ $\le$ $2(\left\vert A_i \right\vert + \left\vert B_i \right\vert)$ $-$ $2\left\vert B_i \right\vert$ $=$ $2\left\vert A_i \right\vert$.
As a result, claim holds.

Now this is a 2-approximation algorithm.
Proof is like follow.

We will show that $\sum\limits_{S} \left\vert F' \cap \delta(S) \right\vert y_S$ $\le$ $2\sum\limits_{S}y_S$ at the beginning and end of the iteration of $\operatorname{while}$ loop.
First of all, $0$ $=$ $\sum\limits_{S} \left\vert F' \cap \delta(S) \right\vert y_S$ $\le$ $2\sum\limits_{S}y_S$ $=$ $0$ at the first iteration.
Now, let's assume that it works for $l - 1$ iterations.
Now let's assume that $\epsilon$ of $y_C$ increased. 
Then, LHS of given inequality will incrase $\sum\limits_{C \in \mathcal{C}} \left\vert F' \cap \delta(C) \right\vert \epsilon$.
RHS of given inequality will incrase $2\sum\limits_{\mathcal{C}}\epsilon$.
Then, LHS $\le$ RHS is true from the fact "$\sum\limits_{C \in \mathcal{C}} \left\vert \delta(C) \cap F' \right\vert$ $\le$ $2 \left\vert \mathcal{C} \right\vert$".
As a result, claim holds.


{: .box-note}
**Reference** David P. Williamson and David B. Shmoys, The Design of Approximation Algorithms.