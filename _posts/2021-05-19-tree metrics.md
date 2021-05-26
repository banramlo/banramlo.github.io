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


{: .box-note}
**Reference** David P. Williamson and David B. Shmoys, The Design of Approximation Algorithms.