---
layout: post
title: Approximation algorithm(1) - SET COVER
gh-repo: daattali/beautiful-jekyll
tags: [algorithm, approximation]
comments: true
use_math: true
---

An optimization problem is a problem to find minimum/maximum possible soltuion.
For example, $\operatorname{SET COVER}$ is a well-known optimization problem.
For a universe set $U = \\{e_1,e_2,e_3,\cdots,e_n$} and subsets of $U$, $S_j(1 \le j \le n)$. 
$\operatorname{SET COVER}$ is a problem to find a collection of subsets which minimize the sum of costs of subset $W_j$ and it contains every element is $U$.
For example, if $U = \\{e_1,e_2,e_3,e_4,e_5$}, $S_1 = \\{e_1,e_2$}, $S_2 = \\{e_3,e_4$}, $S_3 = \\{e_5$}, $S_4 = \\{e_2,e_3$}, $S_5 = \\{e_1$}, $S_6 = \\{e_4$}, $W_1=10$, $W_2=10$, $W_3=1$, $W_4=15$, $W_5=1$, $W_6=1$.
Optimal solution would be $W_3 + W_4 + W_5 + W_6 = 1 + 15 + 1 + 1 = 18$.
Notice that $S_3 \cup S_4 \cup S_5 \cup S_6 = U$.
However, it is known that $\operatorname{SET COVER}$ is a NP-hard.
Which means that requires more than polynomial time if P $\neq$ NP.

Now, here the approximation algorithm works.

$\alpha$-approximation algorithm is an algorithm that solves problem with maximum of $\alpha$ factor difference.
Which means if problem's solution is $\operatorname{OPT}$, solution of $\alpha$-approximation algorithm $ans$ needs to be $ans \le \alpha OPT$.

For the easy of explanation, $\operatorname{SET COVER}$ can be converted like below.
Minimize the $\sum\limits_{j=1}^{m} w_j x_j$ such that $\sum\limits_{j:e_i \in S_j} x_j \ge 1$, $x_j \in \\{0,1$}.
Notice that this kinds of problem is known as integer problem.

However, if we do relaxation, we can do approximation over this.
New problem is like below.

Minimize the $\sum\limits_{j=1}^{m} w_j x_j$ such that $\sum\limits_{j:e_i \in S_j} x_j \ge 1$, $x_j \ge 0$.
Notice that this kinds of problem is known as linear problem.
Which is known to be solvable in the polynomial time.

After solving problem above, we can get $x_j$ as the solution.
However, to make it as a vaild solution, we need to do rounding.
To achieve this, we need to find the maximum number of existance of element in unverse set $e_i$ in $S$s.
For example, if $U = \\{e_1,e_2,e_3,e_4,e_5$}, $S_1 = \\{e_1,e_2$}, $S_2 = \\{e_3,e_4$}, $S_3 = \\{e_5$}, $S_4 = \\{e_2,e_3$}, $S_5 = \\{e_1$}, $S_6 = \\{e_4$}, $W_1=10$, $W_2=10$, $W_3=1$, $W_4=15$, $W_5=1$, $W_6=1$.
Number of existances are like $f_{e_1} = 2$, $f_{e_2} = 2$, $f_{e_3} = 2$, $f_{e_4} = 2$, $f_{e_5} = 1$.
So, maximum of $f_{e_i}$, $f = 2$.
Now, we only set $x_j \ge \frac{1}{f}$ to 1, otherwise to 0.
Algorithm above can be done in polynomial time.

Now, we need to prove that this is a proper $f$-approximation algorithm for original problem.

1. Since, we choosed $x_j \ge \frac{1}{f}$ to 1 and maximum number of existance of element is $f$, we need to choose at least one of such $x_j \ge \frac{1}{f}$.
Otherwise, $\sum\limits_{j|e_i \in S_j} x_j \ge 1$ can't be happen.
Therefore, $\operatorname{ans}$ is a valid solution.

2. Let's assume that answer before rouding to $\operatorname{round}$.
Since $\operatorname{ans}$ choosed only  $x_j \ge \frac{1}{f}$ to 1, $\operatorname{ans} \le f \operatorname{round}$.
Notice that $\operatorname{round}$ will have no $x > 1$.
However $\operatorname{round} \le \operatorname{OPT}$ because we did relaxation.
As a result, $\operatorname{ans} \le f \operatorname{round} \le f\operatorname{OPT}$.
Proof stops in here. 

Now, we made an efficient approach to this $\operatorname{SET COVER}$.

{: .box-note}
**Reference** David P. Williamson and David B. Shmoys, The Design of Approximation Algorithms.