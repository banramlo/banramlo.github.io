---
layout: post
title: Parallel Page rank/BFS
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

<div class="alg">
    $\operatorname{for} i \leftarrow 0,\cdots,|V| - 1$<br>
    <div class="alg">
        $s[i] \leftarrow 1/|V|$<br>
    </div>
    $\operatorname{while} error \le threshold$<br>
    <div class="alg">
        $error \leftarrow 0$<br>
        $\operatorname{for} v \leftarrow 0,\cdots,|V| - 1$<br>
        <div class="alg">
            $o[v] \leftarrow s[v]$<br>
            $s[v] \leftarrow 0$<br>
        </div>
        $\operatorname{for} v \leftarrow 0,\cdots,|V| - 1$<br>
        <div class="alg">
            $\operatorname{for} u \in \delta(v)$<br>
            <div class="alg">
                $s[v] \leftarrow s[v] + \beta \frac{o[u]}{|\delta(u)|}$<br>
            </div>
            $s[v] \leftarrow s[v] + (1 - \beta) \frac{1}{|V|}$<br>
            $error \leftarrow error + |o[v] - s[v]|$<br>
        </div>
    </div>
</div>

If we see the algorithm above, it is known to be parallelizable for each $\operatorname{for}$ loops because there is no dependancy between each $\operatorname{for}$ loops.
As a result, page rank can be parallelized like below.

<div class="alg">
    $\operatorname{for} i \leftarrow 0,\cdots,|V| - 1 \operatorname{in} \operatorname{parellel}$<br>
    <div class="alg">
        $s[i] \leftarrow 1/|V|$<br>
    </div>
    $\operatorname{while} error \le threshold$<br>
    <div class="alg">
        $error \leftarrow 0$<br>
        $\operatorname{for} v \leftarrow 0,\cdots,|V| - 1 \operatorname{in} \operatorname{parellel}$<br>
        <div class="alg">
            $o[v] \leftarrow s[v]$<br>
            $s[v] \leftarrow 0$<br>
        </div>
        $\operatorname{for} v \leftarrow 0,\cdots,|V| - 1 \operatorname{in} \operatorname{parellel}$<br>
        <div class="alg">
            $\operatorname{for} u \in \delta(v) \operatorname{in} \operatorname{parellel}$<br>
            <div class="alg">
                $s[v] \leftarrow s[v] + \beta \frac{o[u]}{|\delta(u)|}$<br>
            </div>
            $s[v] \leftarrow s[v] + (1 - \beta) \frac{1}{|V|}$<br>
        </div>
        $\operatorname{for} v \leftarrow 0,\cdots,|V| - 1 \operatorname{in} \operatorname{reduction}$<br>
        <div class="alg">
            $error \leftarrow error + |o[v] - s[v]|$<br>
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
Notice that it is easy to check about the dependancies by reading the algorithm above and there may exists read dependancy but there is no write dependancy.

## BFS

BFS is a search algorithm which reads vertices from source.
It doesn't need to be a specific algorithm.
It may updates the edge distance from source, it may finds component by checking boolean variable of vertices.
In this case, let's assume that BFS works for set a value of vertex to a distance from the source.
For do this, let's define $\delta(v)$ as the set of neighbor of $v$.

<div class="alg">
    $\operatorname{for} i \leftarrow 0, \cdots, |V| - 1$<br>
    <div class="alg">
        $d[i] = -1$<br>
    </div>
    $Q \leftarrow \emptyset$<br>
    $Q.push(s)$<br>
    $d[s] = 0$<br>
    $\operatorname{while} Q \neq \emptyset$<br>
    <div class="alg">
        $v \leftarrow Q_{top}$<br>
        $Q.pop()$<br>
        $\operatorname{for} u \in \delta(v)$<br>
        <div class="alg">
            $\operatorname{if} d[u] = -1$<br>
            <div class="alg">
                $d[u] = d[v] + 1$<br>
                $Q.insert(u)$
            </div>
        </div>
    </div>
</div>

Now, it can be parallelized per vetices at the same depth.

<div class="alg">
    $\operatorname{for} i \leftarrow 0, \cdots, |V| - 1 \operatorname{in} \operatorname{parellel}$<br>
    <div class="alg">
        $d[i] = -1$<br>
    </div>
    $Q \leftarrow \emptyset$<br>
    $Q.push(s)$<br>
    $d[s] = 0$<br>
    $\operatorname{while} Q \neq \emptyset$<br>
    <div class="alg">
        $Q^n \leftarrow \emptyset$<br>
        $\operatorname{for} v \in Q \text{ with the same depth } \operatorname{in} \operatorname{parellel}$<br>
        <div class="alg">
            $\operatorname{for} u \in \delta(v)$<br>
            <div class="alg">
                $\operatorname{if} d[u] = -1$<br>
                <div class="alg">
                    Critical section<br>
                    <div class="alg">
                        $d[u] = d[v] + 1$<br>
                        $Q^n.insert(u)$
                    </div>
                </div>
            </div>
        </div>
        $Q \leftarrow Q^n$
    </div>
</div>

However, there are two problems at this approach.
One is that we need a critical section to handle every thing.
Therefore, it's not good for many cases.
We may can avoid critical section for $Q$ by using a local queue.
However, we need critical section for $d[u]$ anyway.
Also, there could be potentially many collision for $\delta(v)$.
Therefore many of them is actually waste of performance.

Therefore, there is another approach for BFS known as bottom-up-approach.
To do this, let's define $\delta(v)$ as incoming edges in this case.

<div class="alg">
    $\operatorname{for} i \leftarrow 0, \cdots, |V| - 1 \operatorname{in} \operatorname{parellel}$<br>
    <div class="alg">
        $d[i] = -1$<br>
    </div>
    $Q \leftarrow \emptyset$<br>
    $Q.push(s)$<br>
    $d[s] = 0$<br>
    $\operatorname{while} \text{everything done}$<br>
    <div class="alg">
        $Q^n \leftarrow \emptyset$<br>
        $\operatorname{for} v \in V \operatorname{in} \operatorname{parellel}$<br>
        <div class="alg">
            $\operatorname{if} d[v] = -1$<br>
            <div class="alg">
                $\operatorname{for} u \in \delta(v)$<br>
                <div class="alg">
                    $\operatorname{if} u \in Q$<br>
                    <div class="alg">
                        $d[v] = d[u] + 1$<br>
                        Critical section<br>
                        <div class="alg">
                            $Q^n.insert(v)$
                        </div>
                    </div>
                </div>
            </div>
        </div>
        $Q \leftarrow Q^n$
    </div>
</div>

In this case, we can utilize full computing unit because it can pass many collisons.
Also, this can be parallelized better because there is no critical section to update each node's depth at the moment.
Therefore, this works well than before some times.
However, it is known to be better to use both with some criteria.
Therefore, there is a finite automaton to switching between these two.
It starts with top-down approach first.
If it has lower number of edges to explor then it switches to bottom-up approach.
If it has few vertices to work then it switchs back to top-down approach.
