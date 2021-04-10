---
layout: post
title: Approximation algorithm(9) - Minimizing sum of completion time
gh-repo: daattali/beautiful-jekyll
tags: [algorithm, approximation]
comments: true
use_math: true
---

## Unweighted version

Minimizing the sum of completion time is a typical scheduling problem.
Let's think that there are $n$ works to be done.
With these works, let $r_1, r_2, \cdots, r_n$ as release dates and $p_1, p_2, \cdots, p_n$ as processing times.
Now we will schedule that $n$ works in some order.
With that order, let's define $C_1, C_2, \cdots, C_n$ as times that each work finished.
The problem is to find a schedule that minimize $\sum\limits_{i=1}^n C_i$.
Notice that this machine is a nonpreemptively which means each work can't interrupt other works.

Now we can do a relaxation at this problem by just changing nonpreemptive machine to preemptive machine which means each work may interrupt other works.
If we change it to the preemptive machine, we can solve this problem with an algorithm below.

<div class="algorithm">
    $\operatorname{while}$ there is any work not finished.<br>
    <div class="algorithm">
        $S \leftarrow \{$ work $i$ which didn't finished yet and $r_i \le$ current date $\}$.<br>
        Choose a work $i \in S$ that has the smallest left processing time.<br>
        Process work $i$ until work $i$ is done or new $r_j$ release.<br>
    </div>
</div>
This algorithm is so-called SRPT(shorted remaining processing time) rule.
This algorithm works in a polynomial time because we can update a left processing time and get a minimum processing time by heap.
We also can check whether new $r_j$ released or not by just sorting $r_i$ and checking it later.

However, notice that schedule from preemptive machine can't be run on the nonpreemptive machine.
Therefore, we need to make a strict order for the execution.
Therefore, we will reorder the index of works to the order of $C_i^P$ which $C_i^P$ is the termination time of preemptive machine.
Now after this reordering, we will just do work in the order of $1,2,\cdots,n$.

After this reordering, it will result in $2$-approximation algorithm.
The proof is like follow.

Let $C_i^N$ is the termination time of nonpreemptive machine.
Since, schedule of nonpreemptive machine can be run on preemptive machine, $\sum\limits_{i=1}^n C_i^P \le \operatorname{OPT}$ such that $\operatorname{OPT}$ is an optimal solution.
Now, there is two simple facts that $C^P_i \ge \max\limits_{k=1,\cdots,i}r_k$ and $C^P_i \ge \sum\limits_{k=1}^i p_k$ because it is reordered in the order of $C^P_i$.
Notice that if work $j$ is done then every work $i < j$ should be done from the definition.
Now, let's think about work $i$ in the nonpreemptive machine.
Then we can know that $i$ should finished at most $\max\limits_{k=1,2,\cdots,i}r_k$ $+$ $\sum\limits_{k=1}^i p_k$.
The reason is that work $i$ can only be idle because work $1, 2, \cdots, i - 1$ are still working.

Therefore, $C^N_i$ $\le$ $\max\limits_{k=1,2,\cdots,i}r_k$ $+$ $\sum\limits_{k=1}^i p_k$ $\le$ $2C^P_i$.
Which means $\sum\limits_{i=1}^n C^N_i$ $\le$ $2\sum\limits_{i=1}^n C^P_i$ $\le$ $2\operatorname{OPT}$.

## Weighted version
The weighted version of the problem is simillar with the unweighted version.

Let's think that there are $n$ works to be done.
With these works, let $r_1, r_2, \cdots, r_n$ as release dates, $p_1, p_2, \cdots, p_n$ as processing times and $w_1, w_2, \cdots, w_n$ as weights of each works.
Notice that $w_i \ge 0$ $\forall 1 \le i \le n$
Now we will schedule that $n$ works in some order.
With that order, let's define $C_1, C_2, \cdots, C_n$ as times that each work finished.
The problem is to find a schedule that minimize $\sum\limits_{i=1}^n w_i C_i$.
Notice that this machine is a nonpreemptively which means each work can't interrupt other works.

Now, we can't solve problem with preemptive machine unlike unweighted verison of the problem in polynomail time else P is NP.
Therefore, we will use a relaxation with linear programming in it.
Relaxation of this problem is like below.

Minimize $\sum\limits_{i=1}^n w_i C_i$ such that $C_i \ge r_i + p_i$ $\forall i \in N$ and $\sum\limits_{i \in S} p_iC_i \ge \frac{1}{2}\rho(S)^2$ $\forall S \subset N$ which $N = \\{1, 2, \cdots, n$}.
Objective function is trivial to be "Minimize $\sum\limits_{i=1}^n w_i C_i$" and first constraint is trivial either that "$C_i \ge r_i + p_i$ $\forall i \in N$" from the meaning.
How about the second constraint?
If you think about any $S \subset N$, each works in $S$ will be ordered in an optimal solution.
Therefore, we can imagine that $S^\star$ is a ordered list in the order of an optimal solution from $S$.
Then, <br>
<div style="algin=center;">
        $\sum\limits_{i \in S} p_iC_i$ <br>
        $=$ $\sum\limits_{i \in S^\star} p_iC_i$ <br>
        $\ge$ $\sum\limits_{i,j \in S^\star, j \le i} p_ip_j$ <br>
        $=$ $\frac{1}{2}\sum\limits_{i,j \in S^\star, j \le i} p_ip_j$ + $\frac{1}{2}\sum\limits_{i,j \in S^\star, j \le i} p_ip_j$<br>
        $=$ $\frac{1}{2}\sum\limits_{i,j \in S^\star, j \le i} p_ip_j$ + $\frac{1}{2}\sum\limits_{j,i \in S^\star, i \le j} p_jp_i$<br>
        $=$ $\frac{1}{2}\sum\limits_{i,j \in S^\star, j \le i} p_ip_j$ + $\frac{1}{2}\sum\limits_{j,i \in S^\star, i < j} p_jp_i$ $+$ $\frac{1}{2}\sum\limits_{i \in S^\star} p_ip_i$<br>
        $=$ $\frac{1}{2}\sum\limits_{i,j \in S^\star,} p_ip_j$ + $\frac{1}{2}\sum\limits_{i \in S^\star} p_i^2$ <br>
        $=$ $\frac{1}{2}(\sum\limits_{i \in S^\star} p_i)^2$ + $\frac{1}{2}\sum\limits_{i \in S^\star} p_i^2$ <br>
        $\ge$ $\frac{1}{2}(\sum\limits_{i \in S^\star} p_i)^2$.<br>
</div>
Now let $\rho(S) = \sum\limits_{i \in S} p_i$ then constraint holds.

Now, if we can solve that linear problem in polynomial time.
We can make a strict order by termination time $C_i$ like unweighted version did.
Then that solution should be in $3\operatorname{OPT}$.

Proof is like follow.
Let's define $C^L_i$ as a termination time founded by linear programming.
However we can sort that works in the order of $C^L_i$ so let's assume so.
Which means $C^L_1$ $\le$ $C^L_2$ $\le$ $C^L_3$ $\le$ $\cdots$ $\le$ $C^L_n$.
Let's define one more solution $C^R_i$ as a termination time of nonpreemptive machine's work $i$ with processing order in $C^L_i$.
Then, we can get 2 facts like unweighted version did and we can know other two facts below With these facts.

1. $\sum\limits_{i = 1}^nC^L_i \le \operatorname{OPT}$<br>
2. $C^R_i$ $\le$ $\max\limits_{k=1,2,\cdots,i}r_k$ $+$ $\sum\limits_{k=1}^i p_k$
3. $\max\limits_{k=1,2,\cdots,i}r_k  \le C^L_i$<br>
4. $\frac{1}{2}\sum\limits_{k=1}^i p_k = \frac{1}{2}\rho(\\{1,2,\cdots,i\\}) \le C^L_i$

Third fact is a trivial because $C^L_i$ is ordered in $i$ and this means $C^L_i \ge C^L_j$ $\forall 1 \le j \le i$ and from the constraint $C^L_i \ge C^L_j \ge r_j$ $\forall 1 \le j \le i$.
Simillary, fourth fact can be proven in following way.
From the constaint, $\frac{1}{2}\rho(\\{1,2,\cdots,i\\})^2$ $\le$ $\sum\limits_{j = 1}^{i} p_jC^L_j$.<br>
Then, $\frac{1}{2}\rho(\\{1,2,\cdots,i\\})^2$ $\le$ $\sum\limits_{j = 1}^{i} p_jC^L_j$ $\le$ $\sum\limits_{j = 1}^{i} p_jC^L_i$ $=$ $C^L_i\sum\limits_{j = 1}^{i} p_j$ $=$ $C^L_i\rho(\\{1,2,\cdots,i\\})$.<br>
As a result, $\frac{1}{2}\rho(\\{1,2,\cdots,i\\})$ $\le$ $C^L_i$ by dividing $\rho(\\{1,2,\cdots,i\\})$ for each side of $\frac{1}{2}\rho(\\{1,2,\cdots,i\\})^2$ $\le$ $C^L_i\rho(\\{1,2,\cdots,i\\})$.<br>
Therefore, fact 3, 4 has been proven.<br>
Which means $C^R_i$ $\le$ $\max\limits_{k=1,2,\cdots,i}r_k$ $+$ $\sum\limits_{k=1}^i p_k$ $\le$ $C^L_i + 2C^L_i$ $=$ $3C^L_i$.<br>
Therefore, $\sum\limits_{i=1}^n C^R_i$ $\le$ $\sum\limits_{i=1}^n 3C^L_i$ $\le$ $3\operatorname{OPT}$

We didn't discussed about the running time and it look exponential because the number of contraints is exponential.
However, we still can solve this problem in the polynomial time with ellipsoid method which doesn't bounded in the number of contraints.

To solve this problem with ellipsoid method, we need a seperation oracle that runs in the polynomial time and checks whether a solution is a feasible or not.
If we have a seperation oracle, ellipsoid method can solve the linear programming in $O(poly(log u, n))$ which $u$ is bit in use for saving the data.

However we need to shirnk the number of constraint to check because it is exponential of number of variables.
Therefore, we claim below.

"Let's define $S_1 = \\{1$}, $S_2 = \\{1, 2$}, $\cdots$, $S_n = \\{1,2,\cdots,n$}.
If every variable satisfies for all $S_i$s then that solution statisfies all constraints."

Before proving this, we will check brief 2 facts below.
Notice that S is an arbitrary set in $N$, $a$ is in $S$ and $b$ isn't in S.<br>
1. $\rho(S)^2 - \rho(S - \\{a\\})^2 = 2p_a\rho(S - \\{a\\}) + p_a^2 = p_a(2\rho(S - \\{a\\}) + p_a)$<br>
1. $\rho(S + \\{b\\})^2$ - $\rho(S)^2 =  2p_b\rho(S) + p_b^2 =  p_b(2\rho(S) + p_b)$<br>

Proof is simple that $\rho(S)^2$ $=$ $\rho(S- \\{a\\} + \\{a\\})^2$ $=$ $(\rho(S- \\{a\\}) + p_a)^2$ $=$ $\rho(S - \\{a\\})^2 + 2p_a\rho(S - \\{a\\}) + p_a^2$ and simillar for fact 2.<br>
Now, let's think about $f(S) = \sum\limits_{i \in S}p_iC_i - \frac{1}{2}\rho(S)$.<br>
Then with the fact above, $f(S - \\{a\\}) - f(S)$ $=$ $-p_aC_a + \frac{1}{2}(\rho(S)^2 - \rho(S - \\{a\\})^2)$ $=$ $-p_aC_a + \frac{1}{2}p_a(2\rho(S - \\{a\\}) + p_a)$ $=$ $p_a(-C_a + \rho(S - \\{a\\}) + \frac{1}{2}p_a)$.
Simillary $f(S + \\{b\\}) - f(S)$ $=$ $p_bC_b -\frac{1}{2}(\rho(S + \\{b\\}) - \rho(S))$ $=$ $p_bC_b - \frac{1}{2}p_b(2\rho(S) + p_b)$ $=$ $p_b(C_b - \rho(S) - \frac{1}{2}p_b)$.

As a result, removing $a$ from $S$ will decrease $f(S)$ if $C_a > \rho(S - \\{a\\}) + \frac{1}{2}p_a$ and
adding $b$ to $S$ will decrease $f(S)$ if $C_b < \rho(S) + \frac{1}{2}p_b$.

Now let's assume that every variable satisfies for all $S_i$s but there is some constraint that doesn't matches.
Which means $\sum\limits_{j \in S_i}p_jC_j < \frac{1}{2}\rho(S_i)^2$.
Notice that $f(S_i) < 0$.
Now, let's remove the biggest $j$ in $S_i$ if removing $j$ decreases $f(S_i)$.
Let's define $S_h$ as the termination of such an operation.
Then, it will stop when $C_h \le \rho(S_h - \\{h\\}) + \frac{1}{2}p_h$ for the biggest $j$ in $S_i$.
However we can add any $1 \le j < h$ to decrease $f(S_h)$ because $C_j$ $\le$ $C_h$ $\le$ $\rho(S_h - \\{h\\}) + \frac{1}{2}p_h$ $<$ $\rho(S_h - \\{h\\}) + p_h$ $=$ $\rho(S_h)$ $<$ $\rho(S_h) + \frac{1}{2}p_j$.
If we define $S_e=\\{1,2,\cdots,h$} then, $f(S_e) \le f(S_h) \le f(S_i) < 0$.
However, it can't be true because from the algorithm $f(S_e) = \sum\limits_{i = 1}^e p_iC_i - \frac{1}{2}\rho(S_e) \ge 0$.
As a result, we don't need to see all the contraint but only for $S = \\{1,2,\cdots,i$} is enough.

{: .box-note}
**Reference** David P. Williamson and David B. Shmoys, The Design of Approximation Algorithms.