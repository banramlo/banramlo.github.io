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
Let's define some terms.
1. $r_i^t$ as the value of vertex $i$ at the $t$th iteration such that $\sum\limits_{i \in V}r_i^0 = 1$.
2. $\delta(i)$ as the set of neighbor vetices of vertex $i$.
3. $\beta$ is a convergance ratio which means $0 \le \beta \le 1$.

With terms above, $r_i^{t + 1}$ $=$ $\beta\sum\limits_{j \in \delta(i)}\frac{r_j^t}{|\delta(j)|}$ $+$ $(1 - \beta)\frac{1}{|V|}$.
If we keep compute this, it will converges to some values and that is the result of the page rank.
Notice that if we have $\sum\limits_{i \in V}r_i^t$ $=$ $1$ then it will keep consistant after $t$th iteration either.
Proof is like follow.

$\sum\limits_{i \in V}r_i^{t + 1}$
$=$ $\sum\limits_{i \in V}(\beta\sum\limits_{j \in \delta(i)}\frac{r_j^t}{|\delta(j)|}$ $+$ $(1 - \beta)\frac{1}{|V|})$
$=$ $\beta\sum\limits_{i \in V}\sum\limits_{j \in \delta(i)}\frac{r_j^t}{|\delta(j)|}$ $+$ $(1 - \beta)\sum\limits_{i \in V}\frac{1}{|V|}$
$=$ $\beta\sum\limits_{(i,j) \in E}\frac{r_j^t}{|\delta(j)|}$ $+$ $(1 - \beta)\frac{|V|}{|V|}$
$=$ $\beta\sum\limits_{j \in V}\sum\limits_{i \in \delta(j)}\frac{r_j^t}{|\delta(j)|}$ $+$ $(1 - \beta)$
$=$ $\beta\sum\limits_{j \in V}\frac{r_j^t}{|\delta(j)|}\sum\limits_{i \in \delta(j)}1$ $+$ $(1 - \beta)$
$=$ $\beta\sum\limits_{j \in V}\frac{r_j^t}{|\delta(j)|}|\delta(j)|$ $+$ $(1 - \beta)$
$=$ $\beta\sum\limits_{j \in V}r_j^t$ $+$ $(1 - \beta)$
$=$ $\beta$ $+$ $(1 - \beta)$ $=$ $1$.

<div class="algorithm">
    $\operatorname{for} i \leftarrow 0,\cdots,|V| - 1$<br>
    <div class="algorithm">
        $s[i] \leftarrow 1/|V|$<br>
    </div>
    $\operatorname{while} error \le threshold$<br>
    <div class="algorithm">
        $error \leftarrow 0$<br>
        $\operatorname{for} v \leftarrow 0,\cdots,|V| - 1$<br>
        <div class="algorithm">
            $o[v] \leftarrow s[v]$<br>
        </div>
        $\operatorname{for} v \leftarrow 0,\cdots,|V| - 1$<br>
        <div class="algorithm">
            $s^{old}[v] \leftarrow s[v]$<br>
            $s[v] \leftarrow 0$<br>
            $\operatorname{for} u \in \delta(v)$<br>
            <div class="algorithm">
                $s[v] \leftarrow s[v] + \beta \frac{o[u]}{|\delta(u)|}$<br>
            </div>
            $s[v] \leftarrow s[v] + (1 - \beta) \frac{1}{|V|}$<br>
            $error \leftarrow error + |s^{old}[v] - s[v]|$<br>
        </div>
    </div>
</div>

If we see the algorithm above, it is known to be parallelizable for each $\operatorname{for}$ loops because there is no dependancy between each $\operatorname{for}$ loops.
As a result, page rank can be parallelized like below.

<div class="algorithm">
    $\operatorname{for} i \leftarrow 0,\cdots,|V| - 1 \operatorname{in} \operatorname{parellel}$<br>
    <div class="algorithm">
        $s[i] \leftarrow 1/|V|$<br>
    </div>
    $\operatorname{while} error \le threshold$<br>
    <div class="algorithm">
        $error \leftarrow 0$<br>
        $\operatorname{for} v \leftarrow 0,\cdots,|V| - 1 \operatorname{in} \operatorname{parellel}$<br>
        <div class="algorithm">
            $o[v] \leftarrow s[v]$<br>
        </div>
        $\operatorname{for} v \leftarrow 0,\cdots,|V| - 1 \operatorname{in} \operatorname{parellel}$<br>
        <div class="algorithm">
            $s^{old}[v] \leftarrow s[v]$<br>
            $s[v] \leftarrow 0$<br>
            $\operatorname{for} u \in \delta(v) \operatorname{in} \operatorname{parellel}$<br>
            <div class="algorithm">
                $s[v] \leftarrow s[v] + \beta \frac{o[u]}{|\delta(u)|}$<br>
            </div>
            $s[v] \leftarrow s[v] + (1 - \beta) \frac{1}{|V|}$<br>
        </div>
        $\operatorname{for} v \leftarrow 0,\cdots,|V| - 1 \operatorname{in} \operatorname{reduction}$<br>
        <div class="algorithm">
            $error \leftarrow error + |s^{old}[v] - s[v]|$<br>
        </div>
    </div>
</div>

There are two major parallelization techniques used above.
1. $\operatorname{for} \operatorname{in} \operatorname{parellel}$<br>
2. $\operatorname{for} \operatorname{in} \operatorname{reduction}$<br>

$\operatorname{for} \operatorname{in} \operatorname{parellel}$ means that it can run in the fully parallelized manner.
$\operatorname{for} \operatorname{in} \operatorname{reduction}$ means that it can be run in the fully parallelized manner but it will collect data in the tree order.
First technique usually used for operations that have no dependancies between.
Second technique usually used for there is some dependancies for data but it will accumulates outputs only.
It can be easily checked about the dependancies by reading the algorithm above.
Notice that there may exists read dependancy but there is no write dependancy.

## BFS

