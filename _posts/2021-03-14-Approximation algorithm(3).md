---
layout: post
title: Approximation algorithm(3) - SET COVER
gh-repo: daattali/beautiful-jekyll
tags: [algorithm, approximation]
comments: true
use_math: true
---

{: .box-note}
**Recap** $\operatorname{SET COVER}$ can be LP-relaxed to "Minimize the $\sum\limits_{j=1}^{m} w_j x_j$ such that $\sum\limits_{j:e_i \in S_j} x_j \ge 1$, $x_j \ge 0$ then choose $x_j$ to $1$ which $x_j \ge \frac{1}{f}$ and otherwise to 0". Which is $f$-approximation algorithm. Which $e_i$ denotes each elements, $S_j$ denotes each subsets, $w_j$ denotes costs of each subsets, $x_j$ is the chocie of each subset. With $f$ is the maximum number of existance of elements.

From the above, we can make $f$-approximation algorithm.
However it may arbitrary big in comparison.
Therefore, we prefer the better approximation.

Here are some different approaches to this $\operatorname{SET COVER}$ with $O(ln(n))$-approximation.

## Greedy algorithm

<div class="algorithm">
    $I \leftarrow \emptyset$<br>
    $\hat S_j \leftarrow S_j \forall j$<br>
    $\operatorname{while}$ $I$ is not a set cover $\operatorname{do}$<br>
    <div class="algorithm">
        $l \leftarrow \operatorname{argmin}_{j:\hat S_j \neq \emptyset} \frac{w_j}{|\hat S_j|}$<br>
        $I \leftarrow I \cup$ {$l$}<br>
        $\hat S_j \leftarrow \hat S_j - S_l \forall j$<br>
    </div>
</div>

Algorithm above is a natural algorithm for $\operatorname{SET COVER}$ and it is the $O(ln(n))$-approximation algorithm.

First, it is trivial that it returns a valid solution because it terminates iff returned solution is a $\operatorname{SET COVER}$.
Second, this algorithm runs in polynomial time because, loops ends up in polynomial time and each step works in only polynomial time.

Now, only left part is that proving solution is bounded by $O(ln(n))$ factor of original solution.
First of all, let's define $I$ as the final solution, $w_1, w_2, \cdots, w_{|I|}$ to weights of each start of iteration and $\hat S_1, \hat S_2, \cdots, \hat S_{|I|}$ to uncovered elements in each start of iteration. 
From the fact that we will choose the minimum average weights, $w_j \le \frac{|\hat S_j| - |\hat S_{j + 1}|}{|\hat S_j|}\operatorname{OPT}$.
Which means $\sum\limits_{j \in I} w_j$ $\le$ $\sum\limits_{j = 1}^{|I|} \frac{|\hat S_j| - |\hat S_{j + 1}|}{|\hat S_j|}\operatorname{OPT}$ $\le$ $\operatorname{OPT}\sum\limits_{j \in I}(\frac{1}{|\hat S_j|} + \frac{1}{|\hat S_j - 1|} + \frac{1}{|\hat S_j - 2|} + \cdots + \frac{1}{|\hat S_{j+1}| + 1})$ $=$ $\operatorname{OPT}\sum\limits_{i}^{n}\frac{1}{i}$ $\le$ $\operatorname{OPT}(1 + ln(n))$.

## Random rounding

One another approach is that choosing 1 in probability of itselft.
Minimize the $\sum\limits_{j=1}^{m} w_j x_j$ such that $\sum\limits_{j:e_i \in S_j} x_j \ge 1$, $x_j \ge 0$ then choose $x_j$ to $1$ with probability of $x_j$ and $0$ with probability $1 - x_j$. Notice that This algorithm will not choose $x_j > 1$.

Now, we can't guarantee that it returns good solution. (It may return the solution with all $1$ inside.)
However, we may make a guarantee for average and it is usual for approximation algorithm with random.
$E[\sum\limits_{j=1}^{m} w_j X_j]$ $=$ $\sum\limits_{j=1}^{m} Pr[X_j = 1]$ $=$ $\sum\limits_{j=1}^{m} w_j x_j \le \operatorname{OPT}$.
As a result, it gives a solution which is better than $\operatorname{OPT}$.
However, it never can't better than the optimal solution unless it doesn't cover all elements in the ground set.
Let's check whether it really can't cover all elements.
$Pr[e_i$ not covered$]$ $=$ $\prod\limits_{j:e_i \in S_j} (1 - x_j)$ $\le$ $\prod\limits_{j:e_i \in S_j} e^{-x_j}$ $=$ $e^{-\sum_{j:e_i \in S_j} x_j}$ $\le$ $e^{-1}$
As a result, it will return a solution with non-covered elements in upper bound of $e^{-1}$.
Which means that this algorithm highly likely returns a non-solution.

Therefore, we need some improvements.
What if we try $c \operatorname{ln} n$ times to get $1$?
$Pr[e_i$ not covered$]$ $=$ $\prod\limits_{j:e_i \in S_j} (1 - x_j)^{c \operatorname{ln} n}$ $\le$ $\prod\limits_{j:e_i \in S_j} e^{-x_j (c \operatorname{ln} n)}$ $=$ $e^{-(c \operatorname{ln} n) \sum_{j:e_i \in S_j} x_j}$ $\le$ $\frac{1}{n^c}$.
As a result, $Pr[$ there exsits an uncovered element $]$ $\le$ $\sum\limits_{i=1}^{n}Pr[e_i$ not covered$]$ $\le$ $\frac{1}{n^{c - 1}}$.
Now, it can show that this random algorithm can produce a feasible solution with resonably high probability.

How about the average of the solution?
First of all, let's define $p(x_j)$ as the probability of "$x_j$ is included in the solution".
Then, $p(x_j)$ $=$ $1 - (1-x_j)^{c \operatorname{ln} n}$.
Which makes $p(x_j)'$ $=$ $(c \operatorname{ln} n)(1 - (1-x_j)^{(c \operatorname{ln} n) - 1})$ $\le$ $c \operatorname{ln} n$.
Which also makes $p(x_j)$ $\le$ $(c \operatorname{ln} n) x_j$.
As a result, $E[\sum\limits_{j=1}^m w_j X_j]$ $=$ $\sum\limits_{j=1}^m w_j Pr[X_j = 1]$ $\le$ $\sum_{j=1}^m w_j (c \operatorname{ln} n) x_j$ $=$ $(c \operatorname{ln} n)\sum_{j=1}^m w_j x_j$ $\le$ $(c \operatorname{ln} n) OPT$

This is a summary of approximation algorithm for $\operatorname{SETCOVER}$.
We will see some other techniques with some examples.

{: .box-note}
**Reference** David P. Williamson and David B. Shmoys, The Design of Approximation Algorithms.