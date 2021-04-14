---
layout: post
title: Approximation algorithm(8) - Linear programming
gh-repo: daattali/beautiful-jekyll
tags: [algorithm, approximation]
comments: true
use_math: true
---

Linear programming is constructed with two factors.
1. Objective function which is mizimizing/maximizing target.
2. Constraints Which are requirements for the problem.

For example, minimize $2x + 5y + 6z$ such that $3x + 4y + 3z \ge 5$, $x + y \ge 10, x,y,z \ge 0$.

There are two major methods to solve this.

## Simplex method
Simplex algorithm is based on the fact that an optimal solution exists on one of the intersection of contraints or on one of constarint itself.
The reason is that a fesaible soltuion space is a convex and problem is a linear combination of axis.
<div class="algorithm">
    For a given linear programming problem, choose a feasible solution $V = (v_1, v_2, \cdots, v_n)$.<br>
    $\operatorname{while}$ moving $V$ following one of intersection of contraints makes $v$ better.
    <div class="algorithm">
        Move $V$ to any way which makes $V$ better. 
    </div>
    Return $V$.
</div>
In practice, it is fast algorithm but it can't be guaranteed to be run in polynimal time.
Therefore, there is an alternative method for it.

## Ellipsoid method
Elliposid algorithm uses so-called a partition orcale.
Partition orcale is an orcale that tells us whether is a solution feasible or not in polynoimal time.
If we have such an orcale, we can bipartite solution space to two seperate spaces.
One has an optimal solution and the other has lefts.
With this, we can solve any linear problem in polynomial time.

{: .box-note}
**Reference** David P. Williamson and David B. Shmoys, The Design of Approximation Algorithms.