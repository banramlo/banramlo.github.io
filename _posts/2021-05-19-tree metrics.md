---
layout: post
title: Approximation of metrics by tree metrics
gh-repo: daattali/beautiful-jekyll
tags: [algorithm, approximation]
comments: true
use_math: true
---

For a given vertices $V$ and distance $V \times V \rightarrow \mathcal{R} : d$.
$(V,d)$ is so called a metric if following properties are hold.

1. $d_{uv}$ $\ge$ $0$ for all $u,v$ $\in$ $V$
2. $d_{uv}$ $=$ $0$ iff $u = v$
3. $d_{uv}$ $=$ $d_{vu}$ for all $u,v$ $\in$ $V$
4. (Triangular inequality) $d_{uv}$ $\le$ $d_{uw}$ $+$ $d_{wv}$ for all $u,v,w$ $\in$ $V$

Notice that we say it is a pseudometric if property 2 changes to "$d_{uv}$ $=$ $0$ if $u = v$".

A tree metic $(V', T)$ for a set of vertices $V$ is a tree $T$ defined on a set of nodes $V' \supseteq V$, whoese edges are given non negative lengths.
For $u,v$ $\in$ $V'$, let $T_{uv}$ denote the length of the unique path between $u$ and $v$ in $T$.

Notice that tree matrix is a matric.
It is trivial to be hold for 1 ~ 3.
For 4, if there is a path $u$ to $w$ and $w$ to $v$, then concatinating them is a path from $u$ to $v$.
It will have the length equal or less than sum of each path because there could be a cycle and it can be removed.

Given a metric $d$ on $V$, we say a tree metric $(V', T)$ is a $\operatorname{tree metic embedding}$ of distortion $\alpha$ if $d_{uv}$ $\le$ $T_{uv}$ $\le$ $\alpha d_{uv}$ for all $u,v$ $\in$ $V$.

However, do we even have any approximation for it in every time?
Yes, if we have gigantic distortion.
However, it's no with some distortion.
In fact it is known that there is no tree metric has distortion less than $\frac{n - 1}{8}$ for some metric $d$ on $n$ vertices.
However there is a good theorem.

Given a metric $d$ on $V$ such that $d_{uv}$ $\ge$ $1$ for all $u$ $\neq$ $v$, there exists a randomized algorithm that finds a tree metric $(V', T)$ such that for all $u, v$ $\in$ $V$, $d_{uv}$ $\le$ $T_{uv}$ and $E[T_{uv}]$ $\le$ $O(\ln \left\vert V \right\vert)d_{uv}$.
Notice that this randomized algorithm picks a tree metric from a given graph.
Proof is like follow.

Consider the following algorithm where $B(x, r)$ is a hypersphere with the center at $x$ and radius of $r$.
<div class="alg">
    Pick $r_0$ $\in$ $[\frac{1}{2}, 1)$ uniformly at random<br>
    Let $r_i$ $=$ $2^ir_0$ for $1$ $\le$ $i$ $\le$ $\log_2 \Delta$<br>
    Choose $\Delta$ as the smallest power of two greater than 2$\max_{u,v \in V}d_{uv}$<br>
    Pick a permutation $\pi$ of $v$ uniformly at random<br>
    $\mathcal{C}(\log_2 \Delta) \leftarrow \{V\}$<br>
    Create a node corresponding to $V$ and make it the root node<br>
    $\operatorname{for}$ $i \leftarrow \log_2 \Delta, \log_2 \Delta - 1, \cdots, 1$
    <div class="alg">
        $\mathcal{C}(i - 1) \leftarrow \emptyset$<br>
        $\operatorname{for}$ $C \in \mathcal{C}(i)$
        <div class="alg">
            $S \leftarrow C$<br>
            $\operatorname{for}$ $j \leftarrow 1, 2, \cdots, \left\vert V \right\vert$<br>
            <div class="alg">
                $\operatorname{if}$ $B(\pi(j), r_{i-1})$ $\cap$ $S$ $\neq$ $\emptyset$<br>
                <div class="alg">
                    Add $B(\pi(j), r_{i-1})$ $\cap$ $S$ to $\mathcal{C}(i -1)$<br>
                    $S$ $\leftarrow$ $S$ $-$ $(B(\pi(j), r_{i-1}) \cap S)$
                </div>
            </div>
            Create nodes corresponding to each set in $\mathcal{C}(i -1)$ and attach each node to the node in $\mathcal{C}(i)$ corresponding to its superset by an edge of length $2^i$
        </div> 
    </div> 
</div>

{: .box-note}
**Reference** David P. Williamson and David B. Shmoys, The Design of Approximation Algorithms.