---
layout: post
title: Approximation algorithm(5) - Knapsack
gh-repo: daattali/beautiful-jekyll
tags: [algorithm, approximation]
comments: true
use_math: true
---

The knapsack problem is a well-known problem.
Problem is like below.
There is a set of items $I = \\{1,2,\cdots,n$}, where each item $i$ has a value $v_i > 0$ and a size $s_i > 0$.
Now, knapsack problem is a "Find the maximum possible value with capacity $B$".

There is a well-known dynamic programming algorithm to solve this.
<div class="algorithm">
    $\operatorname{for} j \leftarrow 0, \cdots, B$<br>
    <div class="algorithm">
        $\operatorname{OPT}_{0, j} \leftarrow 0$
    </div>
    $\operatorname{for} i \leftarrow 1, \cdots, n$<br>
    <div class="algorithm">
        $\operatorname{for} j \leftarrow 0, \cdots, B$<br>
        <div class="algorithm">
            $\operatorname{if} w_i > j$<br>
            <div class="algorithm">
                $\operatorname{OPT}_{i, j} \leftarrow \operatorname{OPT}_{i - 1, j}$
            </div>
            $\operatorname{else}$<br>
            <div class="algorithm">
                $\operatorname{OPT}_{i, j} \leftarrow \max(\operatorname{OPT}_{i -1, j}, \operatorname{OPT}_{i - 1, j - w_i} + v_i)$
            </div>
        </div>
    </div>
    $\operatorname{return} OPT_{n, B}$
</div>
In this case, $\operatorname{OPT}_{i,j}$ means an optimal value possible with $1,2,\cdots, i$ items and capacity below $j$.

There is an alternative algorithm to solve this.
<div class="algorithm">
    $V \leftarrow \sum\limits_{i = 1}^{n} v_i$
    $\operatorname{for} j \leftarrow 0, \cdots, V$<br>
    <div class="algorithm">
        $\operatorname{OPT}_{0, j} \leftarrow \infty$
    </div>
    $\operatorname{for} i \leftarrow 1, \cdots, n$<br>
    <div class="algorithm">
        $\operatorname{for} j \leftarrow 0, \cdots, V$<br>
        <div class="algorithm">
            $\operatorname{if} j < v_i$<br>
            <div class="algorithm">
                $\operatorname{OPT}_{i, j} \leftarrow \operatorname{OPT}_{i - 1, j}$
            </div>
            $\operatorname{else}$<br>
            <div class="algorithm">
                $\operatorname{OPT}_{i, j} \leftarrow \max(\operatorname{OPT}_{i -1, j}, \operatorname{OPT}_{i - 1, j - v_i} + s_i)$
            </div>
        </div>
    </div>
    $\operatorname{return}(\max_{j=1}^{W}{j | OPT_{n, j} \le B})$
</div>
In this case, $\operatorname{OPT}_{i,j}$ means the smallest size possible with $1,2,\cdots, i$ items with value below $j$.
Now, it is shown that we have two options.
Algorithm bounded in O(nB) or O(nW).

However, $B$ and $W$ may arbitrary big and running time may become too long.
Now, we will try to make a quantization for it.

## Quantization
<div class="algorithm">
    $M \leftarrow \max_{i \in I} v_i$<br>
    $\mu \leftarrow \epsilon \frac{M}{n}$<br>
    $\operatorname{for} i \leftarrow 1, \cdots, n$<br>
    <div class="algorithm">
        $v_i \leftarrow$ [$\frac{v_i}{\mu}$]
    </div>
    $V \leftarrow \sum\limits_{i = 1}^{n} v_i$
    $\operatorname{for} j \leftarrow 0, \cdots, V$<br>
    <div class="algorithm">
        $\operatorname{OPT}_{0, j} \leftarrow \infty$
    </div>
    $\operatorname{for} i \leftarrow 1, \cdots, n$<br>
    <div class="algorithm">
        $\operatorname{for} j \leftarrow 0, \cdots, V$<br>
        <div class="algorithm">
            $\operatorname{if} j < v_i$<br>
            <div class="algorithm">
                $\operatorname{OPT}_{i, j} \leftarrow \operatorname{OPT}_{i - 1, j}$
            </div>
            $\operatorname{else}$<br>
            <div class="algorithm">
                $\operatorname{OPT}_{i, j} \leftarrow \max(\operatorname{OPT}_{i -1, j}, \operatorname{OPT}_{i - 1, j - v_i} + s_i)$
            </div>
        </div>
    </div>
    $\operatorname{return}(\max_{j=1}^{W}{j | OPT_{n, j} \le B})$
</div>

From the algorithm above, we can see that an approximation algorithm runs in polynomial time.
Let's define $v_i'$ $=$ [$\frac{v_i}{\mu}$].
Then for the new $V$ value, $V'$ $=$ $\sum\limits_{i = 1}^n v_i'$ $\le$ $\sum\limits_{i = 1}^n (\frac{v_i}{\mu})$ $=$ $\sum\limits_{i = 1}^n (\frac{v_i n}{M \epsilon})$ $\le$ $\sum\limits_{i = 1}^n (\frac{n}{\epsilon})$ $=$ $O(n^2/\epsilon)$.

Now, we need to show that it returns at least $(1 - \epsilon)$ of $\operatorname{OPT}$ for show it is a $\operatorname{FPAS}$.
To prove this, we need some facts, 1, $M \le OPT$, 2. $\mu v_i' \le v_i \le \mu(v_i' + 1)$.
Also, we can get $\mu v_i' \ge v_i - \mu$, $\epsilon M = n \mu$.

Now, let $S$ as a set of items which an algorithm above gives and $O$ as a set of items in an optimal solution.
Then, $\sum\limits_{i \in S} v_i$ $\ge$ $\sum\limits_{i \in S} \mu v_i'$ $\ge$ $\sum\limits_{i \in O} \mu v_i'$ $\ge$ $\sum\limits_{i \in O} (v_i - \mu)$ $=$ $\sum\limits_{i \in O}v_i - |O|\mu$ $\ge$ $\sum\limits_{i \in O}v_i - n\mu$ $\=$ $\sum\limits_{i \in O}v_i - \epsilon M$ $=$ $\operatorname{OPT} - \epsilon M$ $\ge$ $\operatorname{OPT} - \epsilon \operatorname{OPT}$ $=$ $(1 - \epsilon)\operatorname{OPT}$.

{: .box-note}
**Reference** David P. Williamson and David B. Shmoys, The Design of Approximation Algorithms.