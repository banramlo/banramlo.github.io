---
layout: post
title: Approximation algorithm(7) - Minimum-degree spanning tree
gh-repo: daattali/beautiful-jekyll
tags: [algorithm, approximation]
comments: true
use_math: true
---

Minimum-degree spanning tree problem is a problem to minimize the degree of the spanning tree from given graph.
Formally, problem is like below.
Given a graph $G = (V,E)$, minimized the $\max_{v \in V} deg(v)$ from a spanning tree $T$ of $G$.
Notice that this is a NP-hard problem.

There is an approximation algorithm for this problem.
Approximation algorithm will work with a local search.
Let $l = \left[ log_2 n \right]$, $d_T(u)$ denotes the degree of $u$ in $T$ and $\Delta(T) = max_{u \in V} d_T(u)$
<div class="algorithm">
    Make an arbitrary spanning tree of $G$.<br>
    $\operatorname{while} \exists (v,w) \not\in T.E$ but $(v,w) \in G.E$
        such that there is a $u$ such that $u$ is a vertex in a cycle $C$ of $T.E$ + $(v,w)$,
            $d_T(u) \ge \Delta(T) - l$ and $max(d_T(v), d_T(w)) \le d_T(u) - 2$<br>
    <div class="algorithm">
        $T \leftarrow T + (v,w) - $ an edge which is incident to $u$ in a cycle $C$.
    </div>
    return $T$
</div>

From the algorithm above, it guarantess that a local search will imporve the solution and it will terminates.
Notice that "$max(d_T(v), d_T(W)) \le d_T(u) - 2$" gurantees that "a local search will imporve the solution" and
"$\operatorname{while} \exists (v,w) \not\in T.E$ but $(v,w) \in G.E$
        such that there is a $u$ such that $u$ is a vertex in a cycle $C$ of $T.E$ + $(v,w)$,
            $d_T(u) \ge \Delta(T) - l$ and $max(d_T(v), d_T(w)) \le d_T(u) - 2$" gurantees that "it will terminates".

Now, let's prove that $\Delta(T) \le 2\operatorname{OPT} + l$ which $\operatorname{OPT}$ is an optimal solution.

Let's assume that we removes $k$ edges from any spanning tree $T$ and re-connecting them again to be a connected graph.
Notice that it will become $k + 1$ distinct components after removing $k$ edges because it is a tree.
Now, let's define the set of one end point of inter-connecting edges between distinct components to $S$.
Then, we have $\operatorname{OPT} \ge \frac{k}{|S|}$.
The reason is like follow.
It should be connected after re-connecting them again and this requires $k$ edges at least.
However, edges can be choosesn in the $S$ only.
Which means each components need to have at least $\frac{k}{|S|}$ edges.

Now, let's think about an output of algorithm $T$ from above.
Let $S_i$ denote the set of nodes of $deg(v) \ge i$ in $T$.
Then there is some $i^{\star}$ $\ge$ $\Delta(T) - l + 1$ such that $|S_{i^{\star}-1}|$ $\le$ $2|S_i^{\star}|$.
The reason is like follow.
If we don't have any $i^{\star}$ such fits above then $|S_{\Delta(T)} - l|$ $>$ $2|S_{\Delta(T)} - l + 1|$ $>$ $2^2|S_{\Delta(T)} - l + 2|$ $>$ $\cdots$ $>$ $2^l|S_{\Delta(T)}|$.
However it can't be ture because $|S_{\Delta(T)} - l|$ $>$ $2^l|S_{\Delta(T)}|$ $\ge$ $2^l$ $\ge$ $n$ from the fact "$|S_{\Delta(T)}| \ge 1$".

Now, let's think about $i \ge \Delta(T) - l + 1$, then we have at leat $(i - 1)|S_i| + 1$ distinct edges of $T$ which are incident to $v \in S_i$.
The reason is like follow.
Since, they have at least $i$ edges for each node.
They will have at least $i|S_i|$ edges.
However, they may have some edges in share which is at most $|S_i| - 1$ because it is a tree.
Therefore, they need to have at least $i|S_i| - (|S_i| - 1)$ $=$ $(i - 1)|S_i| + 1$ edges.

Now, let's think about the situation that we remove all the edges which are incident to $v \in S_i$ from $T$ then all vetices which one end point of vertex's edge is connecting different components are in $S_{i-1}$.
If any edges that connecting different components doesn't incident to $S_{i-1}$ then it should have less than $i - 2$ degree which means that it should be removed from the algorithm.
Notice that we can make a cycle with this edge and one of edges in $S_i$ because each componenets are connected in itself.

From the facts above, $\operatorname{OPT}$ $\ge$ $\frac{k}{\mid S\mid}$ $\ge$ $\frac{(i^{\star}-1)\mid S_{i^{\star}} + 1\mid}{\mid S_{i^{\star}-1} \mid}$ $\ge$ $\frac{(i^{\star}-1)\mid S_{i^{\star}} + 1 \mid}{2\mid S_{i^{\star}}\mid}$ $\ge$ $\frac{i^{\star}-1}{2}$ $\ge$ $\frac{\Delta(T) - l}{2}$.

Let's prove that this algorithm runs in polynomial time.
Let's define a potential function $\Phi$ of tree $T$ such that $\Phi(T)$ $=$ $\sum\limits_{v \in V} 3^{d_T(v)}$.
Notice that $n$ $<$ $\Phi(T)$ $\le$ $n3^{\Delta(T)}\le n3^n$.
After doing a local search once, degree of $v$ and $w$ will increase and $u$ will decrease.
Let's assume that $u$ has a degree $i \ge \Delta(T) -l$.
As a result, potential of $T$ will increase no more than $2(3^{i-1} - 3^{i-2})$ $=$ $4 \times 3^{i-2}$ and will decrease $3^i - 3^{i - 1} = 2 \times 3^{i - 1}$.
Which means potential of $T$ will decrease at least $\frac{2}{9}3^i$ $\ge$ $\frac{2}{9}3^{\Delta(T) - l}$ $=$ $\frac{2}{9 \times 3^{l}}3^{\Delta(T)}$ $\ge$ $\frac{2}{27 \times 3^{log_2 n}}3^{\Delta(T)}$ $\ge$ $\frac{2}{27 \times 4^{log_2 n}}3^{\Delta(T)}$ $=$ $\frac{2}{27n^2}3^{\Delta(T)}$ $\ge$ $\frac{2}{27n^3}\Phi(T)$.
Now, potential decrase at least by a factor of $(1-\frac{2}{27n^3})$ at each iteration.

From the fact above, potential function will be $n$ after at most $\frac{27}{2}n^4 ln 3$ iteration because $(1-\frac{2}{27n^3})^{\frac{27}{2}n^4 ln 3} \times (n3^n)$ $\le$ $e^{-n ln 3} \times (n3^n)$ $=$ $n$.

{: .box-note}
**Reference** David P. Williamson and David B. Shmoys, The Design of Approximation Algorithms.