---
layout: post
title: Approximation of metrics by tree metrics
gh-repo: daattali/beautiful-jekyll
tags: [algorithm, approximation]
comments: true
use_math: true
---

For a given vertices $V$ and distance $V \times V \leftarrow \mathcal{R} : d$.
$(V,d)$ is so called a metric if following properties are hold.

1. $d_{uv}$ $\ge$ $0$ for all $u,v$ $\in$ $V$
2. $d_{uv}$ $=$ $0$ iff $u = v$
3. $d_{uv}$ $=$ $d_{vu}$ for all $u,v$ $\in$ $V$
4. (Triangular inequality) $d_{uv}$ $\le$ $d_{uw}$ $+$ $d_{wv}$ for all $u,v,w$ $\in$ $V$

A tree metic $(V', T)$ for a set of vertices $V$ is a tree $T$ defined on a set of nodes $V' \supseteq V$, whoese edges are given non negative lengths.
For $u,v$ $\in$ $V'$, let $T_{uv}$ denote the length of the unique path between $u$ and $v$ in $T$.

Given a metric $d$ on $V$, we say a tree metric $(V', T)$ is a $\operatorname{tree metic embedding}$ of distortion $\alpha$ if $d_{uv}$ $\le$ $T_{uv}$ $\le$ $\alpha d_{uv}$ for all $u,v$ $\in$ $V$.

However, do we even have any approximation for it in every time?
Yes, if we have gigantic distortion.
However, it's no with some distortion.
In fact it is known that there is no tree metric has distortion less than $\frac{n - 1}{8}$ for some metric $d$ on $n$ vertices.
However there is a good theorem.

Given a metric $d$ on $V$ such that $d_{uv}$ $\ge$ $1$ for all $u$ $\neq$ $v$, there exists a randomized algorithm that finds a tree metric $(V', T)$ such that for all $u, v$ $\in$ $V$, $d_{uv}$ $\le$ $T_{uv}$ and $E[T_{uv}]$ $\le$ $O(\ln \left\vert V \right\vert)d_{uv}$

{: .box-note}
**Reference** David P. Williamson and David B. Shmoys, The Design of Approximation Algorithms.