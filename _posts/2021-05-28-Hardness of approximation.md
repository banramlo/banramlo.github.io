---
layout: post
title: Hardness of approximation
gh-repo: daattali/beautiful-jekyll
tags: [algorithm, approximation]
comments: true
use_math: true
---

MAX-E3SAT is a problem that finds a truth value assignment that satisfies the maximum number of clawes for a given a set of claw which contains exactly three literals.

MAX-2SAT is a problem that finds a truth value assignment that satisfies the maximum number of clawes for a given a set of clawes which contains at most two literals.

Then there exists a $\frac{7}{8}$-approximation algorithm for MAX-E3SAT problem.

Proof is like follow.
If you see all of the clawes and count the existance of each literals and complementary of literals.
Then, we can pick at least half of them to be satisfied by pick $x_i$ or $\overline{x_i}$.
Then, we can cover at least $\frac{7}{8}$ of them because we have three literals for one clawes.

Then, P=NP if there exists a $(\frac{7}{8} + \epsilon)$-approximation algorithm for MAX-E3SAT problem for all constant $\epsilon > 0$.

Proof will be updated later.

Then there exists no $\alpha$-approximation for MAX 2SAT for any constant $\alpha > \frac{433}{440}$ unless P=NP.

Proof is like follow.

First of all, let's think about follow.

For three literals $l1$, $l2$ and $l3$, consider the follwing set of ten clawes in terms of $l1, l2, l3$ and auxiliary variable $y$.

1.  $l1$
2.  $l2$
3.  $l3$
4.  $\overline{l1}\lor\overline{l2}$
5.  $\overline{l2}\lor\overline{l3}$
6.  $\overline{l1}\lor\overline{l3}$
7.  $y$
8.  $l1\lor\overline{y}$
9.  $l2\lor\overline{y}$
10. $l3\lor\overline{y}$

If $l1 \lor l2 \lor l3$ is satisfied, we can choose the value of $y$ so that exactly seven of the ten clawes are satisfied and it is impossible to satisfy more than that.
If $l1 \lor l2 \lor l3$ is not satisfied, we can choose the value of $y$ so that exactly six of the ten clawes are satisfied and it is impossible to satisfy more than that.

Notice that we can have following table with number of satisfied clawes if $y$ is true.

| # true literals   | # true claws in 1~3   | # true claws in 4~6   | # true claws in 7~10          | # true claws in Total       |
| :------           | :------               | :------               | :------                       | :------                     |
| 3                 | 3                     | 0                     | 4                             | 7                           |
| 2                 | 2                     | 2                     | 3                             | 7                           |
| 1                 | 1                     | 3                     | 2                             | 6                           |
| 0                 | 0                     | 3                     | 1                             | 4                           |

If $y$ is false then number of satisfied clawes is like below.

| # true literals   | # true claws in 1~3   | # true claws in 4~6   | # true claws in 7~10          | # true claws in Total       |
| :------           | :------               | :------               | :------                       | :------                     |
| 3                 | 3                     | 0                     | 3                             | 6                           |
| 2                 | 2                     | 2                     | 3                             | 7                           |
| 1                 | 1                     | 3                     | 3                             | 7                           |
| 0                 | 0                     | 3                     | 3                             | 6                           |

As a result, maximum is follow.

| # true literals   | # true claws in 1~3   | # true claws in 4~6   | # true claws in 7~10 (best)   | # true claws in Total(best) |
| :------           | :------               | :------               | :------                       | :------                     |
| 3                 | 3                     | 0                     | 4 ($y$ as true)               | 7                           |
| 2                 | 2                     | 2                     | 3 ($y$ as true/false)         | 7                           |
| 1                 | 1                     | 3                     | 3 ($y$ as false)              | 7                           |
| 0                 | 0                     | 3                     | 3 ($y$ as false)              | 6                           |

Now. think about $m$ clawes that consistes an instance of the MAX E3SAT problem with $n$ varaibles.
We construct a MAX 2SAT instance by follow.

For each $j$th claw in MAX E3SAT problem, make 10 clawes with distinct auxiliary varaible in the algorithm above.
Then, set $l1$, $l2$, $l3$ as each literals used in $j$th claw.

For example, following MAX E3SAT was given.

1. $x_1 \lor x_2 \lor x_3$
2. $\overline{x_2} \lor x_4 \lor x_5$

Then, following is corresponding MAX 2SAT problem.

1.  $x_1$
2.  $x_2$
3.  $x_3$
4.  $\overline{x_1}\lor\overline{x_2}$
5.  $\overline{x_2}\lor\overline{x_3}$
6.  $\overline{x_1}\lor\overline{x_3}$
7.  $y_1$
8.  $x_1\lor\overline{y_1}$
9.  $x_2\lor\overline{y_1}$
10. $x_3\lor\overline{y_1}$
11. $\overline{x_2}$
12. $x_4$
13. $x_5$
14. $x_2\lor\overline{x_4}$
15. $\overline{x_4}\lor\overline{x_5}$
16. $x_2\lor\overline{x_5}$
17. $y_2$
18. $\overline{x_2}\lor\overline{y_2}$
19. $x_4\lor\overline{y_2}$
20. $x_5\lor\overline{y_2}$

Notice that if we make it so then, it shares the optimal solution because MAX 2SAT problem can satisfies 7 of clawes iff corresponding MAX E3SAT problem's claw satisfies and MAX 2SAT problem can satisfies 6 of clawes otherwise which is less than 7. 
For example, if $x_1$ is true and $x_4$ is true then, we can set some $y_1$ and $y_2$ to 14 of 20 becomes true.

Now, run the $\alpha$-approximation algorithm for MAX 2SAT on this instance.

Let's defnine some terminologies.

1. $k^{\star}$ be the number of satisfing clawes of MAX E3SAT instance from the optimal solution.
2. $\overline{k}$ be the number of satisfing clawes of MAX E3SAT instance from the $\alpha$-approximation algorithm's output of MAX 2SAT problem.

Notice that we may have some auxiliary variables $y$ but we will just ignore it for $\overline{k}$.

Then, MAX 2SAT instance's optimal soltuion satisfies $7k^{\star}$ $+$ $6(m - k^{\star})$ clawes.
Which means, $\alpha(7k^{\star}$ $+$ $6(m - k^{\star}))$ $\le$ $7\overline{k}$ $+$ $6(m - \overline{k})$.
Nocie that $0$ $<$ $\alpha$ $\le$ $1$ because this is maximization problem.

As a result, following inequality is true.

$\alpha(7k^{\star}$ $+$ $6(m - k^{\star}))$ $\le$ $7\overline{k}$ $+$ $6(m - \overline{k})$ $\leftrightarrow$
$\alpha(k^{\star}$ $+$ $6m)$ $\le$ $\overline{k}$ $+$ $6m$ $\leftrightarrow$
$\alpha k^{\star}$ $+$ $\alpha 6m$ $\le$ $\overline{k}$ $+$ $6m$ $\leftrightarrow$
$\alpha k^{\star}$ $+$ $\alpha 6m$ $-$ $6m$ $\le$ $\overline{k}$ $\leftrightarrow$
$\alpha k^{\star}$ $+$ $6(\alpha  - 1)m$ $\le$ $\overline{k}$ $\leftrightarrow$
$\alpha k^{\star}$ $-$ $6(1 - \alpha)m$ $\le$ $\overline{k}$.

Now, we already have $\frac{7}{8}$-approximation algorithm.
Therefore, $k^{\star}$ $\ge$ $\frac{7}{8}m$ and $\frac{8}{7}k^{\star}$ $\ge$ $m$.

As a result, $\overline{k}$ $\ge$
$\alpha k^{\star}$ $-$ $6(1 - \alpha)m$ $\ge$
$\alpha k^{\star}$ $-$ $6(1 - \alpha)\frac{8}{7}k^{\star}$ $=$
$(\alpha - 6(1 - \alpha)\frac{8}{7})k^{\star}$ $=$
$(\frac{55}{7}\alpha - \frac{48}{7})k^{\star}$.

Notice that $-$ $6(1 - \alpha)m$ $\le$ $0$.

If there is $\alpha > \frac{433}{440}$,
$\overline{k}$ $\ge$
$(\frac{55}{7}\alpha - \frac{48}{7})k^{\star}$ $>$
$(\frac{55}{7}\frac{433}{440} - \frac{48}{7})k^{\star}$ $=$
$(\frac{7}{8})k^{\star}$.

Now, we show that such $\alpha$-approximation for MAX 2SAT can be used to give $(\frac{7}{8} + \epsilon)$-approximation algorithm for MAX-E3SAT problem for some constant $\epsilon > 0$ and $\alpha > \frac{433}{440}$.
Which is a contradiction.
Therefore, there is no such an approximation algorithm.

{: .box-note}
**Reference** David P. Williamson and David B. Shmoys, The Design of Approximation Algorithms.