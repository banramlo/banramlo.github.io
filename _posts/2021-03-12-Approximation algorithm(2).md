---
layout: post
title: Approximation algorithm(2) - SET COVER
gh-repo: daattali/beautiful-jekyll
tags: [algorithm, approximation]
comments: true
use_math: true
---

{: .box-note}
**Recap** $\operatorname{SET COVER}$ can be LP-relaxed to "Minimize the $\sum\limits_{j=1}^{m} w_j x_j$ such that $\sum\limits_{j:e_i \in S_j} x_j \ge 1$, $x_j \ge 0$ then choose $x_j$ to $1$ which $x_j \ge \frac{1}{f}$ and otherwise to 0". Which is $f$-approximation algorithm. Which $e_i$ denotes each elements, $S_j$ denotes each subsets, $w_j$ denotes costs of each subsets, $x_j$ is the chocie of each subset. With $f$ is the maximum number of existance of elements.

Here are some different approaches to this $\operatorname{SET COVER}$.

## Dual solution
Maximize the $\sum\limits_{i=1}^{n} y_i$ such that $\sum\limits_{i:e_i \in S_j} y_i \le w_j$, $y_i \ge 0$.
This is so-called dual of original LP.
Now, how do we rounding?
We can choose $\sum\limits_{i:e_i \in S_j} y_i = w_j$.

This apporach is like make the sum of cost of elements to expensive possible but every subset can cover costs.
Also, choose only subset fully utilize the cost of own.

Now, let's show this is a $f$-appoximation either.

First, let's show that solution of dual, $I$ is a set cover.
Suppouse that we didn't cover some $e_k$ so for every subset $e_k \in S_j$, $\sum\limits_{i:e_i \in S_j} y_i < w_j $.
Now find the minimum difference $w_j$ which $\epsilon = \min_{j:e_k \in S_j}(w_j - \sum\limits_{i:e_i \in S_j} y^o_i)$.
Then, a new solution with $y^b_k = y^o_k + \epsilon \le w_j$ is still a feasible soltuion.
It means the contradiction which $y^o_k$ was the maximum.
Therefore dual problem should produced a feasible solution.

From the original LP, it can be known that $\sum\limits_{i=1}^{n} y_i \le \sum\limits_{i=1}^{n} (y_i \sum\limits_{j:e_i \in S_j} x_j) = \sum\limits_{i=1}^{n}\sum\limits_{j:e_i \in S_j} y_i x_j = \sum\limits_{j=1}^{m}\sum\limits_{i:e_i \in S_j} y_i x_j = \sum\limits_{j=1}^{m} (x_j \sum\limits_{i: e_i \in S_j} y_i )$ since $\sum\limits_{j:e_i \in S_j} x_j \ge 1$.
From the constrain $\sum\limits_{j=1}^{m} (x_j \sum\limits_{i: e_i \in S_j} y_i) \le \sum\limits_{j=1}^{m} x_j w_j$ is true.
As a result, $\sum\limits_{i=1}^{n} y_i \le \sum\limits_{j=1}^{m} x_j w_j$.
Since it works with any feasible solution $x_j$ and $y_i$, $\sum\limits_{i=1}^{n} y^o_i \le \sum\limits_{j=1}^{m} x^o_j w_j$ is true for a solution of original LP $x^o_j$ and a solution of dual $y^o_i$.
In summary, $\sum\limits_{i=1}^{n} y^o_i \le \sum\limits_{j=1}^{m} x^o_j w_j \le \operatorname{OPT}$.

Now with the fact above, $\sum\limits_{j \in I} w_j$ $=$ $\sum\limits_{j \in I} (\sum\limits_{i:e_i \in S_j} y^o_i)$ from the constraint.
$\sum\limits_{j \in I} (\sum\limits_{i:e_i \in S_j} y^o_i)$ $=$ $\sum\limits_{i=1}^{n}(\sum\limits_{e_i \in S_j, j \in I} y^o_i)$ $=$ $\sum\limits_{i=1}^{n}(|j \in I, e_i \in S_j| y^o_i)$ $\le$ $\sum\limits_{i=1}^{n}f y^o_i$ $\le$ $f\sum\limits_{i=1}^{n}y^o_i$ $\le$ $f \operatorname{OPT}$.
Now we proved this is the $f$-approximation algorithm.

## Primal-dual algorithm
Now, there are some major points in this algorithm.
1. $\sum\limits_{i \in l} y_i \le \operatorname{OPT}$
2. We can make a better solution by choose $y^b_k = y^o_k + \epsilon = w_j$ without lossing valid constraint.
3. We will choose $\sum\limits_{i:e_i \in S_j} y_i = w_j$.

From the above, we can make better algorithmic approach for it.
<div class="alg">
    $y \leftarrow 0$<br>
    $l \leftarrow \emptyset$<br>
    <div class="alg">$\operatorname{while}$ there exists $e_i \not\in \bigcup\limits_{j \in I} S_j \operatorname{do}$<br>
        <div class="alg">Increase the dual variable $y_i$ untill there is some $l$ with $e_i \in S_l$ such that $\sum\limits_{j:e_j \in S_l} y_j = w_l$<br>
        $I \leftarrow I \cup$ {$l$}</div>
    </div>
</div>

From the facts above, we can notice that after inreasing $y_j$, we still have 1.
From the fact 2 and 3, we can choose any $y_j$ to make it still feasible.
Proof is ommited in here because it just follows exact the same process of dual program.
However, it can remove linear programming.

{: .box-note}
**Reference** David P. Williamson and David B. Shmoys, The Design of Approximation Algorithms.