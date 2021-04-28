---
layout: post
title: Parallel graph algorithm(2)
gh-repo: daattali/beautiful-jekyll
tags: [algorithm, concurrency, graph]
comments: true
use_math: true
---

## Page rank
Page rank is an algorithm to determine which vertices are important and which aren't.
For each iteration, each vertex will be updated by nearby vertices.
If we define $r_i^t$ as the value of vertex $i$ at $t$th iteration,
then $r_i^{t + 1} = \belta\sum\limits_{j \in \operatorname{in_neighbor}(i)}r_j^t + (1 - \beta)r_i^t$.

<div class="algorithm">
    Loop untill convergence
    <div class="algorithm">
        for $u$ : all nodes
        <div class="algorithm">
            for $v$ : $\operatorname{in_neighbor}(u)$
            <div class="algorithm">
                $\alpha [u] += 0.85 * \frac{\alpha [v]}{\operatorname{outdeg}(v)}$
            </div>
            $\alpha [u] += \frac{(1 - 0.85)}{|V|}$
        </div>
    </div>
</div>


## BFS

