---
layout: post
title: Approximation algorithm(11) - survivable network design and iterated rounding
gh-repo: daattali/beautiful-jekyll
tags: [algorithm, approximation]
comments: true
use_math: true
---

Survivable network design is a problem that designing a network that can survive from disconnecting some edges.

Let's assume that there is a given undirected graph $G = (V,E)$ with costs $c_e \ge 0$ $\forall e \in E$.
Now, let's define connectivity requirments $r_{ij} \in \mathbb{Z}^{+} \cup \\{0\\}$ for all pairs of vertices $i,j \in V$, where $i \neq j$.
Then problem is a find a minimum cost set of edges $F \subset E$ such that $G' = (V,F)$ has $r_{ij}$ edge distinct paths to connecting $i$ and $j$.

Now, we can make a integer programming over this and it is like follow.
"Minimize $\sum\limits_{e \in E} c_e x_e$ such that $\sum\limits_{e \in \delta(S)} x_e \ge \max\limits_{i \in S, j \not\in S} r_{ij}$ $\forall S \subset V$, $x_e \in \\{0,1\\}$" which $\delta(S)$ denotes the set of edges between $S$ and $V - S$.
Notice that if there is $r_{ij}$ edge distinct paths, we can make a flow of value $r_{ij}$ between $i$ and $j$.
Then, there should be a mincut that corresponding to that flow from the network flow theory.
As a result, constraint above should be fulfilled.
It's the same in the opposite direction.
If we have a such min-cut then we have such flow either.

Now, we can do a linear programming relaxation like we did in the set cover.
"Minimize $\sum\limits_{e \in E} c_e x_e$ such that $\sum\limits_{e \in \delta(S)} x_e \ge \max\limits_{i \in S, j \not\in S} r_{ij}$ $\forall S \subset V$, $0 \le x_e \le 1$".
We can solve this LP in polynomial time with ellipsoid method.
We can construct a seperation orcale like follow.
1. Make a graph that consists of $G = (V,E)$ and set capacity of $E$ as $x_e$.
2. Solve the network flow problem between all pair $i$ and $j$ such that $i \neq j$ and $i,j \in V$.
3. If there is a flow such that the value of flow $f_{ij}$ is less than $r_{ij}$ then it's an infeasible solution otherwise it is feasible.

Now, let's define $f(S) = \max\limits_{i \in S, j \not\in S} r_{ij}$.
Then problem will be shifted to "Minimize $\sum\limits_{e \in E} c_e x_e$ such that $\sum\limits_{e \in \delta(S)} x_e \ge f(S)$ $\forall S \subset V$, $0 \le x_e \le 1$".

If we make $f(S)$ as so then $f(S)$ is so-called weakly supermodular.
To be a weakly supermodular, there are some requirements.
1. $f(\emptyset) = f(U) = 0$ for ground set $U$.
2. one of $f(A) + f(B)$ $\le$ $f(A \cap B) + f(A \cup B)$ or $f(A) + f(B)$ $\le$ $f(A - B) + f(B - A)$ should be true.

Proof is like follow.
First, it is trivial that $f(\emptyset) = f(V) = 0$ because there is no such an edge between $V$ and $\emptyset$.
Before we prove the second requirments, $f(A) = f(V - A)$ is trivial either.
Also, we have $max(f(A), f(B)) \ge f(A \cup B)$.
Because if we think about $i \in A \cup B, j \not\in A \cup B$ then it should be one of "$i \in A, j \not\in A$ or $i \in B, j \not\in B$".

Now, we can make 4 property of $f$ from the fact above.

1. $f(A)$ $\le$ $\max(f(A - B), f(A \cap B))$
2. $f(A)$ $=$ $f(V - A)$ $\le$ $\max(f(B - A),$ $f(V - (A \cup B)))$ $=$ $\max(f(B - A),$ $f(A \cup B))$
3. $f(B)$ $\le$ $\max(f(B - A), f(A \cap B))$
4. $f(B)$ $=$ $f(V - B)$ $\le$ $\max(f(A - B),$ $f(V - (A \cup B)))$ $=$ $\max(f(A - B),$ $f(A \cup B))$

Notice that $(A - B)$ $\cup$ $(A \cap B)$ $=$ $A$, $(V - (A \cup B))$ $\cup$ $(B - A)$ $=$ $V$ $-$ $A$, 
$(B - A)$ $\cup$ $(A \cap B)$ $=$ $B$ and $(V- (A \cup B))$ $\cup$ $(A - B)$ $=$ $V$ $-$ $B$.

This implies 4 inequalities.

1. $f(A) + f(B)$ $\le$ $\max(f(A - B), f(A \cap B))$ $+$ $\max(f(B - A), f(A \cap B))$
2. $f(A) + f(B)$ $\le$ $\max(f(A - B), f(A \cap B))$ $+$ $\max(f(A - B), f(A \cup B))$
3. $f(A) + f(B)$ $\le$ $\max(f(B - A), f(A \cup B))$ $+$ $\max(f(B - A), f(A \cap B))$
4. $f(A) + f(B)$ $\le$ $\max(f(B - A), f(A \cup B))$ $+$ $\max(f(A - B), f(A \cup B))$

Then, we can make possible inequality for weakly supermodular in any case.
1. $f(A) + f(B)$ $\le$ $\max(f(A - B), f(A \cap B))$ $+$ $\max(f(B - A), f(A \cap B))$ $=$ $f(A - B)$ $+$ $f(B - A)$ if $f(A - B), f(B - A)$ $\ge$ $f(A \cap B)$
2. $f(A) + f(B)$ $\le$ $\max(f(A - B), f(A \cap B))$ $+$ $\max(f(A - B), f(A \cup B))$ $=$ $f(A \cap B)$ $+$ $f(A \cup B)$ if $f(A \cap B), f(A \cup B)$ $\ge$ $f(A - B)$
3. $f(A) + f(B)$ $\le$ $\max(f(B - A), f(A \cup B))$ $+$ $\max(f(B - A), f(A \cap B))$ $=$ $f(A \cap B)$ $+$ $f(A \cup B)$ if $f(A \cap B), f(A \cup B)$ $\ge$ $f(B - A)$
4. $f(A) + f(B)$ $\le$ $\max(f(B - A), f(A \cup B))$ $+$ $\max(f(A - B), f(A \cup B))$ $=$ $f(A - B)$ $+$ $f(B - A)$ if $f(A - B), f(B - A)$ $\ge$ $f(A \cup B)$

Notice that if all requirements is false then all following should be true.

1. $\min(f(A - B), f(B - A))$ $<$ $f(A \cap B)$
2. $\min(f(A \cap B), f(A \cup B))$ $<$ $f(A - B)$
3. $\min(f(A \cap B), f(A \cup B))$ $<$ $f(B - A)$
4. $\min(f(A - B), f(B - A))$ $<$ $f(A \cup B)$

Then, it is a contraction because of following reasoning.

$\min(f(A - B), f(B - A))$ $<$ $\min(f(A \cap B), f(A \cup B))$ from 1, 4<br>
$\min(f(A \cap B), f(A \cup B))$ $<$ $\min(f(A - B), f(B - A))$ from 2, 3<br>
$\min(f(A \cap B), f(A \cup B))$ $<$ $\min(f(A - B), f(B - A))$ $<$ $\min(f(A \cap B), f(A \cup B))$ from above two.<br>
As a result, $\min(f(A \cap B), f(A \cup B))$ $<$ $\min(f(A \cap B), f(A \cup B))$ and it can't be true.
Therefore, $f$ is a weakly supermodular.

Now, there is a nice property of a weakly supermodular $f$.
There exists an edge $e \in E$ such that $x_e \ge \frac{1}{2}$ if we solve linear problem "Minimize $\sum\limits_{e \in E} c_e x_e$ such that $\sum\limits_{e \in \delta(S)} x_e \ge f(S)$ $\forall S \subset V$, $0 \le x_e \le 1$" for any weakly supermodular $f$.
Proof will be followed at the end of the post.

From the fact above, we can construct an algorithm.

<div class="algorithm">
    $F \leftarrow \emptyset$<br>
    $i \leftarrow 1$<br>
    $\operatorname{while}$ $F$ is not a feasible solution $\textbf{do}$<br>
    <div class = "algorithm">
        Solve "Minimize $\sum\limits_{e \in E - F} c_e x_e$ such that $\sum\limits_{e \in \delta(S), e \in E - F} x_e \ge f_i(S) = f(S) - |\delta(S) \cap F|$ $\forall S \subset V$, $0 \le x_e \le 1$"<br>
        $F_i \leftarrow \{ e \in E - F \vert x_e \ge \frac{1}{2} \}$<br>
        $F \leftarrow F \cup F_i$<br>
        $i \leftarrow i + 1$
    </div>
    $\textbf{return} \text{ } F$
</div>

Notice that it is so-called $\operatorname{iterative rounding}$ because it uses LP-relxation to extend solution and iterates it many times.

If the algorithm above terminates, solution should be feasible.
Now, we will show that solution will be in $2\operatorname{OPT}$ and it terminates.

First of all, we will show "If we define $z(e) \ge 0$ for all $e \in E$ and define $z(E) = \sum\limits_{e \in E}z(e)$ then 
$z(\delta(A)) + z(\delta(B))$ $\ge$ $z(\delta(A \cup B)) + z(\delta(A \cap B))$ and $z(\delta(A)) + z(\delta(B))$ $\ge$ $z(\delta(A - B)) + z(\delta(B - A))$ for any $A, B \subset V$".

Proof is like follow.
If you think about the category of edges in $\delta(A)$, it will be one of follows.
1. $i \in A - B$, $j \in V - (A \cup B)$
2. $i \in A \cap B$, $j \in V - (A \cup B)$
3. $i \in A - B$, $j \in B - A$
4. $i \in A \cap B$, $j \in B - A$

It's the same for the B either.
Therefore, $z(\delta(A)) + z(\delta(B))$ will be sum of $z(e)$ in following 8 categories.
1. $i \in A - B$, $j \in V - (A \cup B)$
2. $i \in A \cap B$, $j \in V - (A \cup B)$
3. $i \in A - B$, $j \in B - A$
4. $i \in A \cap B$, $j \in B - A$
5. $i \in B - A$, $j \in V - (A \cup B)$
6. $i \in A \cap B$, $j \in V - (A \cup B)$
7. $i \in B - A$, $j \in A - B$
8. $i \in A \cap B$, $j \in A - B$

If you think about the category of edges in $\delta(A \cup B)$, it will be one of follows.
1. $i \in A - B$, $j \in V - (A \cup B)$
2. $i \in A \cap B$, $j \in V - (A \cup B)$
3. $i \in B - A$, $j \in V - (A \cup B)$

If you think about the category of edges in $\delta(A \cap B)$, it will be one of follows.
1. $i \in A \cap B$, $j \in V - (A \cup B)$
2. $i \in A \cap B$, $j \in B - A$
3. $i \in A \cap B$, $j \in A - B$

Therefore, $z(\delta(A \cup B)) + z(\delta(A \cap B))$ will be sum of $z(e)$ in following 6 categories.
1. $i \in A - B$, $j \in V - (A \cup B)$
2. $i \in A \cap B$, $j \in V - (A \cup B)$
3. $i \in B - A$, $j \in V - (A \cup B)$
4. $i \in A \cap B$, $j \in V - (A \cup B)$
5. $i \in A \cap B$, $j \in B - A$
6. $i \in A \cap B$, $j \in A - B$

Now, we can do a mapping from this categories of $z(\delta(A \cup B)) + z(\delta(A \cap B))$ to one of 8 categories of $z(\delta(A)) + z(\delta(B))$.
$1 \rightarrow 1$, $2 \rightarrow 2$, $3 \rightarrow 5$, $4 \rightarrow 6$, $5 \rightarrow 4$, $6 \rightarrow 8.$
Now we have category 3, 7 lefts.
As a result, $z(\delta(A)) + z(\delta(B))$ $\ge$ $z(\delta(A \cup B)) + z(\delta(A \cap B))$.

Like above, we can do the same thing for $z(\delta(A)) + z(\delta(B))$ $\ge$ $z(\delta(A - B)) + z(\delta(B - A))$.

If you think about the category of edges in $\delta(A - B)$, it will be one of follows.
1. $i \in A - B$, $j \in V - (A \cup B)$
2. $i \in A - B$, $j \in A \cap B$
3. $i \in A - B$, $j \in B - A$

If you think about the category of edges in $\delta(B - A)$, it will be one of follows.
1. $i \in B - A$, $j \in V - (A \cup B)$
2. $i \in B - A$, $j \in A \cap B$
3. $i \in B - A$, $j \in A - B$

Therefore, $z(\delta(A - B)) + z(\delta(B - A))$ will be sum of $z(e)$ in following 6 categories.
1. $i \in A - B$, $j \in V - (A \cup B)$
2. $i \in A - B$, $j \in A \cap B$ $\leftrightarrow$ $i \in A \cap B$, $j \in A - B$
3. $i \in A - B$, $j \in B - A$
4. $i \in B - A$, $j \in V - (A \cup B)$
5. $i \in B - A$, $j \in A \cap B$ $\leftrightarrow$ $i \in A \cap B$, $j \in B - A$
6. $i \in B - A$, $j \in A - B$

We can map $1 \rightarrow 1$, $2 \rightarrow 8$, $3 \rightarrow 3$, $4 \rightarrow 5$, $5 \rightarrow 4$, $6 \rightarrow 7$.
Now we have category 2, 6 lefts.
As a result, $z(\delta(A)) + z(\delta(B))$ $\ge$ $z(\delta(A - B)) + z(\delta(B - A))$.

As a summary, both followings are true.

1. $z(\delta(A)) + z(\delta(B))$ $\ge$ $z(\delta(A \cup B)) + z(\delta(A \cap B))$
2. $z(\delta(A)) + z(\delta(B))$ $\ge$ $z(\delta(A - B)) + z(\delta(B - A))$


Now, let $z_F(e) = 1$ if $e \in F$ and $z_F(e) = 0$ otherwise.

Then, $f_i(S) = f(S) - |\delta(S) \cap F| = f(S) - z_F(\delta(S))$.
As a result $f_i(S)$ should fulfill one of follows.
1. $f_i(A) + f_i(B)$ $=$ $f(A) + f(B) - z_F(\delta(A)) - z_F(\delta(B))$ $\le$ $f(A \cup B) + f(A \cap B) - z_F(\delta(A \cup B)) - z_F(\delta(A \cap B))$ $=$ $f_i(A \cup B) + f_i(A \cap B)$.
2. $f_i(A) + f_i(B)$ $=$ $f(A) + f(B) - z_F(\delta(A)) - z_F(\delta(B))$ $\le$ $f(A - B) + f(B - A) - z_F(\delta(A - B)) - z_F(\delta(B - A))$ $=$ $f_i(A - B) + f_i(B - A)$.

Now, we showed that $f_i$ is a weakly supermodular.

Therefore, we can found at least one edge such that $x_e \ge \frac{1}{2}$ for any $f_i$.
Now if we can show that "Minimize $\sum\limits_{e \in E - F} c_e x_e$ such that $\sum\limits_{e \in \delta(S), e \in E - F} x_e \ge f_i(S) = f(S) - |\delta(S) \cap F|$ $\forall S \subset V$, $0 \le x_e \le 1$" can be solved in polynomial time, it will the end of the proof for polynomial execution time for the algorithm.
Notice that we can do this at most $O(\left\vert E \right\vert)$.

We still can use network flow algorithm to solve this.
Therefore, we will make a seperation orcale like below.
1. Make a graph that consists of $G = (V,E)$ and set capacity of $E - F$ as $x_e$ and capacity of $F$ as 1.
2. Solve the network flow problem between all pair $i$ and $j$ such that $i \neq j$ and $i,j \in V$.
3. If there is a flow such that the value of flow $f_{ij}$ is less than $r_{ij}$ then it's an infeasible solution otherwise it is feasible.

Notice that if it is feasible then it means $\sum\limits_{e \in \delta(S)} f_e + |\delta(S) \cap F| \ge r_{ij}$ which $\delta(S)$ denotes the set of edges between $S$ and $V - S$ in $E - F$.
As a result, $\sum\limits_{e \in \delta(S)} f_e \ge r_{ij} - |\delta(S) \cap F|$.
If there is some $i, j$ such that flow between $i$ and $j$ is less than $r_{ij}$ then there should be $S$ such that $\sum\limits_{e \in \delta(S)} f_e + |\delta(S) \cap F|< r_{ij}$.
As a result, we can use this as a seperation orcale.

Now only left thing to show is that solution is in $2\operatorname{OPT}$.
To show this, we will prove generalized version of the claim.
The claim is 
"For any weakly supermodular function $f$, let's define $x^i$ as the solution of LP in $i$th iteration.
If we can solve iterative rounding algorithm above in $k$ iteration then solution of iterative rounding $\operatorname{ANS}$ $=$ $\sum\limits_{e \in F} c_e$ is in $2\sum\limits_{e \in E} c_e x_e^1$".
Notice that this implies $\operatorname{ANS}$ $=$ $\sum\limits_{e \in F} c_e$ $\le$ $2\sum\limits_{e \in E} c_e x_e^1$ $\le$ $2\operatorname{OPT}$. 

We will show this in the inductive method.

If $k = 1$, we will solve "Minimize $\sum\limits_{e \in E} c_e x_e$ such that $\sum\limits_{e \in \delta(S), e \in E} x_e \ge f(S)$ $\forall S \subset V$, $0 \le x_e \le 1$" once and that's all.
From the above, we will select $\\{x_e : x_e \ge \frac{1}{2}\\}$.
As a result, $\sum\limits_{e \in F_1} c_e$ $\le$ $2\sum\limits_{e \in F_1} c_e x_e^1$.

If $k \ge 2$, we have three facts follow.
First, $\sum\limits_{e \in F_1} c_e$ $\le$ $2\sum\limits_{e \in F_1} c_e x_e^1$ is true because we selected $x_e^1 \ge \frac{1}{2}$ in $x_e^1$.

Also, if we consider $\\{x_e^1 | e \in E - F_1\\}$ $=$ $\Lambda$ then $\Lambda$ is a feasible solution of LP in the second iteration either.
The reason is like follow.
Second constraint is trivial to be hold because $0 \le x_e^1 \le 1$.
First constraint holds either because of follows.
$\sum\limits_{e \in \delta(S), e \in E - F_1} x_e^1$ $=$ 
$\sum\limits_{e \in \delta(S), e \in E} x_e^1 - \sum\limits_{e \in \delta(S), e \in F_1} x_e^1$ $\ge$ 
$f_1(S) - |\delta(S) \cap F_1|$ $=$
$f_2(S)$.
Therefore, $\Lambda$ is a feasible solution.
As a result, $\sum\limits_{e \in E - F_1} c_e x_e^1$ $\ge$ $\sum\limits_{e \in E - F_1} c_e x_e^2$.

For the last, we can use iterative rounding for $E - F_1$ and $f_2(S)$ because it will terminted in $k - 1$ iterations.
Notice that we've proved that $f_2$ is a weakly supermodular.
As a result, $\sum\limits_{e \in F - F_1} c_e$ $\le$ $2\sum\limits_{e \in E - F_1} c_e x_e^2$.

In a summary, we have three facts.
1. $\sum\limits_{e \in F_1} c_e$ $\le$ $2\sum\limits_{e \in F_1} c_e x_e^1$
2. $\sum\limits_{e \in E - F_1} c_e x_e^2$ $\le$ $\sum\limits_{e \in E - F_1} c_e x_e^1$
3. $\sum\limits_{e \in F - F_1} c_e$ $\le$ $2\sum\limits_{e \in E - F_1} c_e x_e^2$.

If we combine three facts above,
$\sum\limits_{e \in F} c_e$ $=$ 
$\sum\limits_{e \in F - F_1} c_e + \sum\limits_{e \in F_1} c_e$ $\le$ 
$2\sum\limits_{e \in E - F_1} c_e x_e^2 + 2\sum\limits_{e \in F_1} c_e x_e^1$ $\le$
$2\sum\limits_{e \in E - F_1} c_e x_e^1 + 2\sum\limits_{e \in F_1} c_e x_e^1$ $=$
$2\sum\limits_{e \in E} c_e x_e^1$.

Now proof stops here for survivable network design problem.

However, we need to show one following fact.
There exists an edge $e \in E$ such that $x_e \ge \frac{1}{2}$ if we solve linear problem "Minimize $\sum\limits_{e \in E} c_e x_e$ such that $\sum\limits_{e \in \delta(S)} x_e \ge f(S)$ $\forall S \subset V$, $0 \le x_e \le 1$" for any weakly supermodular $f$.

To show this proof, we need some definitions to make a proof.

1. $\chi_E$ is a vector such that $(\chi_E)_e$ $=$ $\cases{ 1, e \in E, x_e > 0\cr 0, \text{otherwise}}$
2. $A$ and $B$ are intersecting if all of $A \cap B$, $A - B$ and $B - A$ are not empty.
3. $A$ is tight if $\sum\limits_{e \in \delta(A)} x_e = f(A)$ for $A \in V$.
4. A collection of sets $\mathcal{L}$ is $\operatorname{laminor}$ if no pair of sets in $\mathcal{L}$ is intersecting.
5. $\operatorname{Span}(\mathcal{L})$ $=$ $\operatorname{Span}\\{\chi_{\delta(S)} \mid S \in \mathcal{L}\\}$

Now, we need some $\operatorname{Lemma}$s.

If $A$ and $B$ are tight and intersecting, at least one of the following is true.
1. $A \cap B$, $A \cup B$ are both tight and $\chi_{\delta(A \cap B)} + \chi_{\delta(A \cup B)}$ $=$ $\chi_{\delta(A)} + \chi_{\delta(B)}$ 
2. $A - B$, $B - A$ are both tight and $\chi_{\delta(A - B)} + \chi_{\delta(B - A)}$ $=$ $\chi_{\delta(A)} + \chi_{\delta(B)}$ 

Proof is like follow. one of $f(A) + f(B)$ $\le$ $f(A \cap B) + f(A \cup B)$ or $f(A) + f(B)$ $\le$ $f(A - B) + f(B - A)$ is true because $f$ is a weakly supermodular.
Then, there are two cases.
If we think about the first case, we can do a reasoning follow.
$\sum\limits_{e \in \delta(A \cap B)} x_e$ $+$ $\sum\limits_{e \in \delta(A \cup B)} x_e$ $\ge$ $f(A \cap B) + f(A \cup B)$ because of constraints of linear problem.
$f(A \cap B) + f(A \cup B)$ $\ge$ $f(A) + f(B)$ because $f$ is a weakly super modular.
$f(A) + f(B)$ $=$ $\sum\limits_{e \in \delta(A)} x_e$ $+$ $\sum\limits_{e \in \delta(B)} x_e$ becacuse $A$ and $B$ is tight.
As a result, $\sum\limits_{e \in \delta(A \cap B)} x_e$ $+$ $\sum\limits_{e \in \delta(A \cup B)} x_e$ $\ge$ $\sum\limits_{e \in \delta(A)} x_e$ $+$ $\sum\limits_{e \in \delta(B)} x_e$.

With this fact, $\sum\limits_{e \in \delta(A \cap B)} x_e$ $+$ $\sum\limits_{e \in \delta(A \cup B)} x_e$ $\ge$ $\sum\limits_{e \in \delta(A)} x_e$ $+$ $\sum\limits_{e \in \delta(B)} x_e$ $\ge$ $\sum\limits_{e \in \delta(A \cap B)} x_e$ $+$ $\sum\limits_{e \in \delta(A \cup B)} x_e$ if we use the fact follow "If we select $z(e) \ge 0$ for all $e \in E$ and define $z(E) = \sum\limits_{e \in E}z(e)$ then 
$z(\delta(A)) + z(\delta(B))$ $\ge$ $z(\delta(A \cup B)) + z(\delta(A \cap B))$ and $z(\delta(A)) + z(\delta(B))$ $\ge$ $z(\delta(A - B)) + z(\delta(B - A))$ for any $A, B \subset V$".

As a result, 
$\sum\limits_{e \in \delta(A \cap B)} x_e$ $+$ $\sum\limits_{e \in \delta(A \cup B)} x_e$ $=$
$f(A \cap B) + f(A \cup B)$ $=$
$f(A) + f(B)$ $=$
$\sum\limits_{e \in \delta(A)} x_e$ $+$ $\sum\limits_{e \in \delta(B)} x_e$ $=$
$\sum\limits_{e \in \delta(A \cap B)} x_e$ $+$ $\sum\limits_{e \in \delta(A \cup B)} x_e$.
Which means that both $A \cap B$, $A \cup B$ are tight.
More over, if we recap the process of prooving the statement of $z$, there were 8 categories of edges and 2 are lefts in each cases.

1. $i \in A - B$, $j \in V - (A \cup B)$
2. $i \in A \cap B$, $j \in V - (A \cup B)$
3. $i \in A - B$, $j \in B - A$
4. $i \in A \cap B$, $j \in B - A$
5. $i \in B - A$, $j \in V - (A \cup B)$
6. $i \in A \cap B$, $j \in V - (A \cup B)$
7. $i \in B - A$, $j \in A - B$
8. $i \in A \cap B$, $j \in A - B$

Now we have category 2, 6 lefts for $\delta(A - B)$ and $\delta(B - A)$.
Similarly we have category 3, 7 lefts for $\delta(A \cup B)$ and $\delta(A \cap B)$.

However, $x_e$ for edges in category 2, 6 should be $0$ because $\sum\limits_{e \in \delta(A \cap B)} x_e$ $+$ $\sum\limits_{e \in \delta(A \cup B)} x_e$ $=$ $\sum\limits_{e \in \delta(A)} x_e$ $+$ $\sum\limits_{e \in \delta(B)} x_e$ and $0 \le x_e \le 1$. Then, we can know that $\delta(A \cap B)$, $\delta(A \cup B)$ and $\delta(A)$, $\delta(B)$ should have the same set of edges. As a result, $\chi_{\delta(A \cap B)} + \chi_{\delta(A \cup B)}$ $=$ $\chi_{\delta(A)} + \chi_{\delta(B)}$.

Simillarly, both $A - B$, $B - A$ are tight and $\chi_{\delta(A - B)} + \chi_{\delta(B - A)}$ $=$ $\chi_A + \chi_B$ should be hold in the second case.

Now, we will claim below with the lemma above.
Then, there is $\mathcal{L} \subset 2^{V}$ which satisfies 4 things.
1. $S$ is tight for all $S \in \mathcal{L}$
2. $\\{\chi_{\delta(S)}\\}_{S \in \mathcal{L}}$ are linear independent.
3. $\left\vert \mathcal{L} \right\vert$ $=$ $\left\vert E \right\vert$
4. $\mathcal{L}$ is $\operatorname{laminor}$.

Proof is like follow.
Let $\mathcal{T} \subset 2^{V}$ be the family of all tight sets and $\mathcal{L}$ as a maximal $\operatorname{laminor}$ subfamily of $\mathcal{T}$.
Which means $\mathcal{L}$ will not be a $\operatorname{laminor}$ if we add anything more to $\mathcal{L}$.

First, we will claim $\operatorname{Span}(\mathcal{T})$ $\subset$ $\operatorname{Span}(\mathcal{L})$ to show $\operatorname{Span}(\mathcal{T})$ $=$ $\operatorname{Span}(\mathcal{L})$.
Notice that $\operatorname{Span}(\mathcal{L})$ $\subset$ $\operatorname{Span}(\mathcal{T})$ is trivial because $\mathcal{L} \subset \mathcal{T}$.

Let's assume not then there are some $S$ $\in$ $\mathcal{T}$ such that $\chi_{\delta(S)}$ $\not\in$ $\operatorname{Span}(\mathcal{L})$.
Between possible $S$s, let's assume that we've choose $S$ such that $S$ has the fewest intersecting sets in $\mathcal{L}$.
However, $S$ should intersect with at least one set in $\mathcal{L}$.
The reason is that we can add $S$ to $\mathcal{L}$ because it doesn't change $\mathcal{L}$ to be not $\operatorname{laminor}$ if it don't intersect.
However, it's contradiction to "$\mathcal{L}$ is maximal $\operatorname{laminor}$".

Now, we can find $T \in \mathcal{L}$ such that $T$ is intersecting with $S$.
Then one of two need to be true because both $S$, $T$ are tight because $T \in \mathcal{L} \subset \mathcal{T}$ and $S \in \mathcal{T}$.

1. $S \cap T$, $S \cup T$ are both tight and $\chi_{\delta(S \cap T)} + \chi_{\delta(S \cup T)}$ $=$ $\chi_{\delta(S)} + \chi_{\delta(T)}$ 
2. $S - T$, $T - S$ are both tight and $\chi_{\delta(S - T)} + \chi_{\delta(T - S)}$ $=$ $\chi_{\delta(S)} + \chi_{\delta(T)}$ 

Let's assume that it's the case 1.
Then both $\chi_{\delta(S \cap T)}$, $\chi_{\delta(S \cup T)}$ can't be in $\operatorname{Span}(\mathcal{L})$ at the same time.

Proof is like follow.
$\chi_{\delta(S)}$ $=$ $\chi_{\delta(S \cap T)}$ + $\chi_{\delta(S \cup T)}$ - $\chi_{\delta(T)}$ and $\chi_{\delta(S)}$ should be in $\operatorname{Span}(\mathcal{L})$ if $\chi_{\delta(S \cup T)}$, $\chi_{\delta(S \cap T)}$ are in $\operatorname{Span}(\mathcal{L})$ at the same time.
It's the same of case 2 because $\chi_{\delta(S)}$ $=$ $\chi_{\delta(S - T)}$ + $\chi_{\delta(T)}$ - $\chi_{\delta(T - S)}$. 

Now, let's think about $X$ such that $\chi_{\delta(X)} \not\in \operatorname{Span}(\mathcal{L})$ and $X \in \\{S \cap T, S \cup T\\}$ in the first case or $\\{S - T, T - S\\}$ in the second case.
Then, $S$ and $Y$ are intersecting for any $Y \in \mathcal{L}$ such that $Y$ is intersecting with $X$.

Proof is like follow.
Notice that $Y$ should be one of three followings because $Y \in \mathcal{L}$, $T \in \mathcal{L}$.
1. $Y \cap T = \emptyset$
2. $T - Y = \emptyset$
3. $Y - T = \emptyset$

Notice that all of followings are true.
1. $X \cap Y \neq \emptyset$
2. $X - Y \neq \emptyset$
3. $Y - X \neq \emptyset$
4. $S \cap T \neq \emptyset$
5. $S - T \neq \emptyset$
6. $T - S \neq \emptyset$

If $X = S \cap T$, $Y$ should fulfill one of three followings.
1. $Y \cap T = \emptyset$<br>
    $Y$ can't intersect with $X$ because $Y \cap X$ $\subseteq$ $Y \cap T$ $=$ $\emptyset$.<br>
    Therefore, such $Y$ doesn't exist.<br>
2. $T - Y = \emptyset$<br>
    $Y$ can't intersect with $X$ because $X - Y$ $\subseteq$ $T - Y$ $=$ $\emptyset$.<br>
    Therefore, such $Y$ doesn't exist.<br>
3. $Y - T = \emptyset$<br>
    $\emptyset$ $\neq$ $X \cap Y$ $\subseteq$ $S \cap Y$.<br>
    $\emptyset$ $\neq$ $X - Y$ $\subseteq$ $S - Y$.<br>
    $\emptyset$ $\neq$ $Y - X$ $=$ $Y - (S \cap T)$ $=$ $(Y - S)$ $\cup$ $(Y - T)$ $=$ $Y - S$.<br>
    As a result, $S$ intersects with $Y$.<br>

If $X = S \cup T$, $Y$ should fulfill one of three followings.
1. $Y \cap T = \emptyset$<br>
    $\emptyset$ $\neq$ $X \cap Y$ $=$ $(S \cup T) \cap Y$ $=$ $(S \cap Y) \cup (T \cap Y)$ $=$ $S \cap Y$.<br>
    $\emptyset$ $\neq$ $S \cap T$ $=$ $(S \cap T) - (S \cap (T \cap Y))$ $=$ $(S \cap T) - ((S \cap T) \cap Y)$ $=$ $(S \cap T) - Y$ $\subseteq$ $((S \cap T) - Y) \cup ((S - T) - Y)$ $=$ $S - Y$.<br>
    $\emptyset$ $\neq$ $Y - X$ $\subseteq$ $Y - S$.<br>
    As a result, $S$ intersects with $Y$.<br>
2. $T - Y = \emptyset$<br>
    $\emptyset$ $\neq$ $S \cap T$ $=$ $S \cap ((T \cap Y) \cup (T - Y))$ $=$ $S \cap (T \cap Y)$ $=$ $S \cap T \cap Y$ $=$ $(S \cap Y) \cap T$ $\subseteq$ $S \cap Y$.<br>
    $\emptyset$ $\neq$ $X - Y$ $=$ $(S \cup T) - Y$ $=$ $(S - Y) \cup (T - Y)$ $=$ $S - Y$.<br>
    $\emptyset$ $\neq$ $Y - X$ $\subseteq$ $Y - S$.<br>
    As a result, $S$ intersects with $Y$.<br>
3. $Y - T = \emptyset$<br>
    $Y$ can't intersect with $X$ because $Y - X$ $\subseteq$ $Y - T$ $=$ $\emptyset$.<br>
    Therefore, such $Y$ doesn't exist.<br>

If $X = S - T$, $Y$ should fulfill one of three followings.<br>
1. $Y \cap T = \emptyset$<br>
    $\emptyset$ $\neq$ $X \cap Y$ $=$ $(S - T) \cap Y$ $\subseteq$ $S \cap Y$.<br>
    $\emptyset$ $\neq$ $X - Y$ $\subseteq$ $S - Y$.<br>
    $\emptyset$ $\neq$ $Y - X$ $=$ $Y - (S - T)$ $=$ $Y - (Y \cap (S - T))$ $=$ $Y - ((Y \cap S) - (Y \cap T))$ $=$ $Y - (Y \cap S)$ $=$ $Y - S$.<br>
    As a result, $S$ intersects with $Y$.<br>
2. $T - Y = \emptyset$<br>
    $\emptyset$ $\neq$ $X \cap Y$ $=$ $(S - T) \cap Y$ $=$ $(S \cap  Y) - (T \cap Y)$ $\subseteq$ $S \cap Y$.<br>
    $\emptyset$ $\neq$ $X - Y$ $\subseteq$ $S - Y$.<br>
    $\emptyset$ $\neq$ $T - S$ $\subseteq$  $(Y \cup T) - S$ $=$ $(Y \cup (T - Y)) - S$ $=$ $(Y - S) \cup ((T - Y) - S)$ $=$ $Y - S$.<br>
    As a result, $S$ intersects with $Y$.<br>
3. $Y - T = \emptyset$<br>
    $Y$ can't intersect with $X$ because $X \cap Y$ $=$ $(S - T) \cap Y$ $=$ $(S \cap Y) - (T \cap Y)$ $=$ $(S \cap Y) - ((T \cap Y) \cup (Y - T))$ $\subseteq$ $((S \cap Y) \cup (Y - S)) - ((T \cap Y) \cup (Y - T))$ $=$ $Y - Y$ $=$ $\emptyset$.<br>
    Therefore, such $Y$ doesn't exist.<br>

If $X = T - S$, $Y$ should fulfill one of three followings.
1. $Y \cap T = \emptyset$<br>
    $Y$ can't intersect with $X$ because $X \cap Y$ $=$ $(T - S) \cap Y$ $\subseteq$ $T \cap Y$ $=$ $\emptyset$.<br>
    Therefore, such $Y$ doesn't exist.<br>
2. $T - Y = \emptyset$<br>
    $Y$ can't intersect with $X$ because $X - Y$ $=$ $(T - S) - Y$ $\subseteq$ $T - Y$ $=$ $\emptyset$.<br>
    Therefore, such $Y$ doesn't exist.<br>
3. $Y - T = \emptyset$<br>
    $\emptyset$ $\neq$ $Y - X$ $=$ $Y - (T - S)$ $=$ $Y - ((T \cup (Y - T)) - S)$ $=$ $Y - ((Y \cup T) - S)$ $=$ $Y - ((Y \cup (T - Y)) - S)$ $\subseteq$ $Y - (Y - S)$ $=$ $S \cap Y$.<br>
    $\emptyset$ $\neq$ $S - T$ $=$ $S - (T \cup (Y - T))$ $=$ $S - (Y \cup T)$ $=$ $S - (Y \cup (T - Y))$ $\subseteq$ $S - Y$.<br>
    $\emptyset$ $\neq$ $X \cap Y$ $=$ $(T - S) \cap Y$ $=$ $(T - (S \cap T)) \cap Y$ $=$ $(T \cap Y) - ((S \cap T) \cap Y)$ $=$ $(T \cap Y) - (S \cap (T \cap Y))$ $=$ $(T \cap Y) - S$ $=$ $((T \cap Y) \cup (Y - T)) - S$ $=$ $Y - S$.<br>
    As a result, $S$ intersects with $Y$.<br>

As a result, $S$ and $Y$ are intersecting for any $Y \in \mathcal{L}$ such that $Y$ is intersecting with $X$.

Now, let's think about $X$.
Then, $X$ should have strictly fewer intersecting sets with $\mathcal{L}$ than $S$.
The reason is like follow.
If $X$ is intersecting with $Y \in \mathcal{L}$ then, $S$ does so either.
However, there is at least one set such that intersects with $S$ but not with $X$ which is $T$.
Notice that $X \in \\{S \cap T, S \cup T, S - T, T - S\\}$ and
$(S \cap T) - T = \emptyset$,
$T - (S \cup T) = \emptyset$,
$(S - T) \cap T = \emptyset$,
$(T - S) - T= \emptyset$.
However, this is contradiction that we've choosed $S$ such that has $S$ has the fewest intersecting sets in $\mathcal{L}$.

Therefore, such an $S$ can't exists and $\operatorname{Span}(\mathcal{T})$ $=$ $\operatorname{Span}(\mathcal{L})$ is true.
Now, we've showend that there is at least one of a maximal $\operatorname{laminor}$ subfamily of $\mathcal{T}$ namely $\mathcal{L}$ such that fulfills following two.
1. $S$ is tight for all $S \in \mathcal{L}$
2. $\mathcal{L}$ is $\operatorname{laminor}$.

Now, we can find some set $S \in \mathcal{T}$ which is lineary dependent from other vectors.
Then, we can just remove it without losing property above.
Now, we keep doing it untill every vectors are lineary independent and let the result to be $\mathcal{L}^\star$.
Then, $\mathcal{L}^\star$ should have $\left\vert E \right\vert$ independent vectors because they are all independent and $\operatorname{Span}(\mathcal{L}^\star)$ Should be still $\operatorname{Span}(\mathcal{T})$ because we removed only lineary dependent vectors.
Notice that each vector has $\left\vert E \right\vert$ elements inside.
Therefore, $\mathcal{L}^\star$ fulfills every property below.
1. $S$ is tight for all $S \in \mathcal{L}^\star$
2. $\\{\chi_{\delta(S)}\\}_{S \in \mathcal{L}^\star}$ are linear independent.
3. $\left\vert \mathcal{L}^\star \right\vert$ $=$ $\left\vert E \right\vert$
4. $\mathcal{L}^\star$ is $\operatorname{laminor}$.

Now, we will show the following is true.

There exists an edge $e \in E$ such that $x_e \ge \frac{1}{2}$ if we solve linear problem "Minimize $\sum\limits_{e \in E} c_e x_e$ such that $\sum\limits_{e \in \delta(S)} x_e \ge f(S)$ $\forall S \subset V$, $0 \le x_e \le 1$" for any weakly supermodular $f$.

To show this, let's think about such an $\mathcal{L}$ satisfies 4 properties from given LP and let's assume that $0 < x_e < \frac{1}{2}$ for all $e \in E$.
Now, let's define set $A$ is bigger than $B$ when $B \subset A$.
Then we can construct a forest since $\mathcal{L}$ is a $\operatorname{laminor}$.
Notice that one of following should be true for $X, Y \in \mathcal{L}$.
1. $X \cap Y$ $=$ $\emptyset$
2. $X - Y$ $=$ $\emptyset$ $\leftrightarrow$ $X \subseteq Y$
3. $Y - X$ $=$ $\emptyset$ $\leftrightarrow$ $Y \subseteq X$

From the fact above, we will construct a forest like follow.

1. Select biggest sets in $\mathcal{L}$ and make them to the roots.
2. Select biggest sets in $S$ for any root $S$ and make them to child of $S$.
3. Keep do this recursively untill every set is in the forest.

Notice that $X$ is the child of $S$ if and only if there is no set $Y$ exists such that $Y$ is the child of $S$ but $X$ is child of $Y$ either.
It means there is no double depth child relation.

With this $\mathcal{L}$, we can pass some costs to the sets.
Let's pass the costs like follow.

1. Pass $x_e$ to set $X$ if $u$ or $v$ is in $X$ and $X$ is the smallest set among such sets for any edge $e = (u,v)$.
2. Pass $1 - 2x_e$ to set $X$ if both $u$ and $v$ are in $X$ and $X$ is the smallest set among such sets for any edge $e = (u,v)$.

Then, select a root of forest $S$.
With this $S$, let's define child of $S$ as $C_1, C_2, \cdots, C_n$ and $C = \bigcup\limits_{k = 1}^{n} C_k$.
Then, we can define 4 categories for edge $e \in (\delta(S) \cup \delta(C))$.

1. $E_{cc}$ is the set of edges $e \in E$ such that one end point of edge is in $C_i$ and other is in $C_j$ for $i \neq j$.
2. $E_{cp}$ is the set of edges $e \in E$ such that one end point of edge is in $C_i$ and other is in $S - C$.
3. $E_{co}$ is the set of edges $e \in E$ such that one end point of edge is in $C_i$ and other is in $V - S$.
4. $E_{po}$ is the set of edges $e \in E$ such that one end point of edge is in $S - C$ and other is in $V - S$.

If we count every cost gain for $S$ from each categories, it is like follow.

1. $1 - 2x_e$ from $E_{cc}$.
2. $1 - 2x_e + x_e = 1 - x_e$ from $E_{cp}$.
3. Nothing from $E_{co}$.
4. $x_e$ from $E_{po}$.

Then, the total costs $S$ gain is $\left\vert E_{cc} \right\vert - 2x(E_{cc}) + \left\vert E_{cp} \right\vert - x(E_{cp}) + x(E_{po})$ $=$ $\left\vert E_{cc}\right\vert + \left\vert E_{cp} \right\vert - 2x(E_{cc}) - x(E_{cp}) + x(E_{po})$.

Notice that $E_{cc} \cup E_{cp} \cup E_{po} \neq \emptyset$.
Proof is like follow.

Let's assume not then $E_{cc} = E_{cp} = E_{po} = \emptyset$.
Which means $x(\delta(S))$ $=$ $x(E_{po} + E_{co})$ $=$ $x(E_{co})$ $=$ $x(E_{co} \cup E_{cc} \cup E_{cp})$ $=$ $x(\delta(C))$ $=$ $x(\delta(\bigcup\limits_{k = 1}^{n} C_k))$ $=$ $\sum\limits_{k = 1}^n x(\delta(C_k))$ because $C_i \cap C_j = \emptyset$.
Notice that $C_i$ is the sibling of $C_j$ in the forest.
However, $x(\delta(S))$ $=$ $\sum\limits_{k = 1}^n x(\delta(C_k))$ can't be true because it's a contradiction for the fact that $\chi_{\delta(S)}$ and $\chi_{\delta(C_i)}$s are linear independent.
Therefore, $\left\vert E_{cc} \right\vert - 2x(E_{cc}) + \left\vert E_{cp} \right\vert - x(E_{cp}) + x(E_{po}) > 0$.
Notice that every value is positive because we assumed $0 < x_e < \frac{1}{2}$.

Now, let's think about the categories of edges for $E_{cc}, E_{cp}, E_{po}$.
Then $x(\delta(S)) = x(E_{po}) + x(E_{co})$ and $x(\delta(C)) = x(E_{cp}) + 2x(E_{cc}) + x(E_{co})$.
Reason is like follow.

1. For $\delta(S)$, all the edges should be one of $E_{co}$ or $E_{po}$ because one of end point should be in $V - S$.
2. For $\delta(C)$, there are three type of edges like just described.
    One for from $C_i$ to $V - S$.
    One for from $C_i$ to $S - C$.
    One for from $C_i$ to $C_j$ for $i \neq j$.
    However, $x(\delta(C))$ will count twice for "One for from $C_i$ to $C_j$ for $i \neq j$".

As a result, $\left\vert E_{cc} \right\vert + \left\vert E_{cp} \right\vert - 2x(E_{cc}) - x(E_{cp}) + x(E_{po})$ $=$ $\left\vert E_{cc} \right\vert + \left\vert E_{cp} \right\vert  + x(E_{po}) + x(E_{co}) - (x(E_{cp}) + 2x(E_{cc}) + x(E_{co}))$
$=$ $\left\vert E_{cc} \right\vert + \left\vert E_{cp} \right\vert + x(\delta(S)) - x(\delta(C))$.
However, $\left\vert E_{cc} \right\vert + \left\vert E_{cp} \right\vert + x(\delta(S)) - x(\delta(C))$ should be an integer because all of elements in the equation are intergers.
Therefore, each $S$ should have at least 1 costs.
Which means total cost of $\mathcal{L} \ge \left\vert E \right\vert$ since $\left\vert \mathcal{L} \right\vert = \left\vert E \right\vert$.
However, total cost of $\mathcal{L} < \left\vert E \right\vert$ is true because of following reasons.

Each edges passes at most $x_e + x_e + (1 - 2x_e) = 1$ costs to sets.
Therefore, total cost of $\mathcal{L} \le \left\vert E \right\vert$.
However, there is at least one edge that outgoing of $S$.
If there is no such an edge, $S$ should contain every vertices and it means $S = V$.
However, $\delta(S) = \emptyset$ because there is no edge between $S$ and $S - V = V - V = \emptyset$.
Which means $S$ can't be in $\mathcal{L}$ because $\\{\chi_{\delta(S)}\\}_{S \in \mathcal{L}}$ are linear independent.
As a result, total cost of $\mathcal{L} < \left\vert E \right\vert.

Therefore, it's a contradiction with the fact above.
As a result, assumption that "$0 < x_e < \frac{1}{2}$ for all $e \in E$" is false.
Therefore, there should be at least $x_e$ shuch that $x_e \ge \frac{1}{2}$ or $x_e = 0$.
However, if we think about the LP with removing $x_e$s than solution of problem should be the same with before.
At the same time, there should be at least one such an $x_e$ that $x_e \ge \frac{1}{2}$ or $x_e = 0$.
However, that should be the case of $x_e \ge \frac{1}{2}$ because we just removed every $x_e = 0$.
Therefore, claim holds.

{: .box-note}
**Reference** David P. Williamson and David B. Shmoys, The Design of Approximation Algorithms.