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

Notice that this randomized algorithm picks a tree metric from a givn graph.

{: .box-note}
**Reference** David P. Williamson and David B. Shmoys, The Design of Approximation Algorithms.