---
layout: post
title: Approximation algorithm(11) - survivable network design and iterated rounding
gh-repo: daattali/beautiful-jekyll
tags: [algorithm, approximation]
comments: true
use_math: true
---

Survivable network design is a problem that designing a network that can survive from disconnecting some edges.

Let's assume there is a given undirected graph $G = (V,E)$ with costs $c_e \ge 0$ $\forall e \in E$.
Now, let's define connectivity requirments $r_{ij} \in \mathbb{Z}^{+} \cup \\{0\\}$ for all pairs of vertices $i,j \in V$, where $i \neq j$.
Then problem is a find a minimum cost set of edges $F \subset E$ such that $G' = (V,F)$ has $r_{ij}$ edge distinct paths to connecting $i$ and $j$.

Now, we can do a integer programming relaxation over this and it is like follow.
"Minimize $\sum\limits_{e \in E} c_e x_e$ such that $\sum\limits_{e \in \delta(S)} x_e \ge \max\limits_{i \in S, j \not\in S} r_{ij}$ $\forall S \subset V$, $x_e \in \\{0,1\\}$" which $\delta(S)$ denotes the set of edges between $S$ and $V - S$.
Notice that if there is $r_{ij}$ edge distinct paths, we can make flow of value $r_{ij}$ between $i$ and $j$.
Then, there should be a mincut that corresponding to that flow from the network flow theory.
As a result, constraint above should fullfilled.
It's the same in the opposite direction.
If we have a such min-cut then we have such flow either.

Now, we can do a linear programming relaxation like we did in the set cover.
"Minimize $\sum\limits_{e \in E} c_e x_e$ such that $\sum\limits_{e \in \delta(S)} x_e \ge \max\limits_{i \in S, j \not\in S} r_{ij}$ $\forall S \subset V$, $0 \le x_e \le 1$".
We can solve this LP in polynomial time with ellipsoid method.
We can construct seperation orcale like follow.
1. Make a graph that consists of $G = (V,E)$ and set capacity of $E$ as $x_e$.
2. Solve the network flow problem between all pair $i$ and $j$ such that $i \neq j$ and $i,j \in V$.
3. If there is a flow such that the value of flow $f_{ij}$ is less than $r_{ij}$ then it's an infeasible solution otherwise it is feasible.

Now, let's define $f(S) = \max\limits_{i \in S, j \not\in S} r_{ij}$.
Then problem will be shifted to "Minimize $\sum\limits_{e \in E} c_e x_e$ such that $\sum\limits_{e \in \delta(S)} x_e \ge f(S)$ $\forall S \subset V$, $0 \le x_e \le 1$".

If we make $f(S)$ as so then $f(S)$ is so-called weakly supermodular.
To be a weakly supermodular, there are some requirements.
1. $f(\emptyset) = f(U) = 0$ for ground set $U$.
2. one of $f(A) + f(B) \le f(A \cap B) + f(A \cup B)$ or $f(A) + f(B) \le f(A - B) + f(B - A)$ should be true.

Proof is like follow.
First, it is trivial that $f(\emptyset) = f(V) = 0$ because there are no such an edge between $V$ and $\emptyset$.
Before we prove the second requirments, $f(A) = f(V - A)$ is trivial either.
Also, we have $max(f(A), f(B)) \ge f(A \cup B)$.
Because if we think about $i \in A \cup B, j \not\in A \cup B$ then it should be one of "$i \in A, j \not\in A$ or $i \in B, j \not\in B$".

Now, we can make 4 property of $f$ from the fact above.

1. $f(A) \le \max(f(A - B), f(A \cap B))$
2. $f(A) = f(V - A) \le \max(f(B - A), f(V - (A \cup B))) = \max(f(B - A), f(A \cup B))$
3. $f(B) \le \max(f(B - A), f(A \cap B))$
4. $f(B) = f(V - B) \le \max(f(A - B), f(V - (A \cup B))) = \max(f(A - B), f(A \cup B))$

Notice that $(A - B) \cup (A \cap B) = A$, $(V - (A \cup B)) \cup (B - A) = V - A$, $(B - A) \cup (A \cap B) = B$ and $(V - (A \cup B)) \cup (A - B) = V - B$.
This implies 4 inequalities.
1. $f(A) + f(B) \le \max(f(A - B), f(A \cap B)) + \max(f(B - A), f(A \cap B))$
2. $f(A) + f(B) \le \max(f(A - B), f(A \cap B)) + \max(f(A - B), f(A \cup B))$
3. $f(A) + f(B) \le \max(f(B - A), f(A \cup B)) + \max(f(B - A), f(A \cap B))$
4. $f(A) + f(B) \le \max(f(B - A), f(A \cup B)) + \max(f(A - B), f(A \cup B))$

Then, we can make possible inequality for weakly supermodular in any case.
1. $f(A) + f(B) \le \max(f(A - B), f(A \cap B)) + \max(f(B - A), f(A \cap B)) = f(A - B) + f(B - A)$ if $f(A - B), f(B - A) \ge f(A \cap B)$
2. $f(A) + f(B) \le \max(f(A - B), f(A \cap B)) + \max(f(A - B), f(A \cup B)) = f(A \cap B) + f(B \cup A)$ if $f(A \cap B), f(B \cup A) \ge f(A - B)$
3. $f(A) + f(B) \le \max(f(B - A), f(A \cup B)) + \max(f(B - A), f(A \cap B)) = f(A \cap B) + f(B \cup A)$ if $f(A \cap B), f(B \cup A) \ge f(B - A)$
4. $f(A) + f(B) \le \max(f(B - A), f(A \cup B)) + \max(f(A - B), f(A \cup B)) = f(A - B) + f(B - A)$ if $f(A - B), f(B - A) \ge f(A \cup B)$
Notice that if $f(A \cap B), f(B \cup A) \ge f(A - B)$, $f(A \cap B), f(B \cup A) \ge f(B - A)$ doesn't right then at least one of $f(A - B), f(B - A) \ge f(A \cap B)$ or $f(A - B), f(B - A) \ge f(A \cup B)$ should be true.
As a result, $f$ is a weakly supermodular

Now, there is some nice property of a weakly supermodular $f$.
There exists an edge $e \in E$ such that $x_e \ge \frac{1}{2}$ if we solve linear problem "Minimize $\sum\limits_{e \in E} c_e x_e$ such that $\sum\limits_{e \in \delta(S)} x_e \ge f(S)$ $\forall S \subset V$, $0 \le x_e \le 1$" for any weakly supermodular $f$.

From the fact above, we can construct an algorithm.

<div class="algorithm">
    $F \leftarrow \emptyset$<br>
    $i \leftarrow 1$<br>
    $\operatorname{while}$ $F$ is not a feasible solution $\textbf{do}$<br>
    <div class = "algorithm">
        Solve "Minimize $\sum\limits_{e \in E - F} c_e x_e$ such that $\sum\limits_{e \in \delta(S), e \in E - F} x_e \ge f_i(S) = f(S) - |\delta(S) \cap F|$ $\forall S \subset V$, $0 \le x_e \le 1$"<br>
        $F_i \leftarrow \{ e \in E - F : x_e \ge \frac{1}{2} \}$<br>
        $F \leftarrow F \cup F_i$<br>
        $i \leftarrow i + 1$
    </div>
    $\textbf{return} \text{ } F$
</div>

Notice that it is so-called $\operatorname{iterative rounding}$ because it uses LP-relxation to extend solution and iterates it many times.

If the algorithm above terminates, solution should be feasible.
Now, we will show that solution will be in $2\operatorname{OPT}$ and it terminates.

First of all, we will show "If we select $z_e \ge 0$ for all $e \in E$ and define $z(E) = \sum\limits_{e \in E}z_e$ then 
$z(\delta(A)) + z(\delta(B)) \ge z(\delta(A \cup B)) + z(\delta(A \cap B))$ and $z(\delta(A)) + z(\delta(B)) \ge z(\delta(A - B)) + z(\delta(B - A))$ for any $A, B \subset V$."

Proof is like follow.
If you think about the category of edges in $\delta(A)$, it will be one of follows.
1. $i \in A - B$ $j \in V - (A \cup B)$
2. $i \in A \cap B$ $j \in V - (A \cup B)$
3. $i \in A - B$ $j \in B - A$
4. $i \in A \cap B$ $j \in B - A$

It's the same for the B either.
Therefore, $z(\delta(A)) + z(\delta(B))$ will be sum of $z_e$ in following 8 categories.
1. $i \in A - B$ $j \in V - (A \cup B)$
2. $i \in A \cap B$ $j \in V - (A \cup B)$
3. $i \in A - B$ $j \in B - A$
4. $i \in A \cap B$ $j \in B - A$
5. $i \in B - A$ $j \in V - (A \cup B)$
6. $i \in A \cap B$ $j \in V - (A \cup B)$
7. $i \in B - A$ $j \in A - B$
8. $i \in A \cap B$ $j \in A - B$<br>
Notice that there is a duplicated categories(2 and 6).

If you think about the category of edges in $\delta(A \cup B)$, it will be one of follows.
1. $i \in A - B$ $j \in V - (A \cup B)$
2. $i \in A \cap B$ $j \in V - (A \cup B)$
3. $i \in B - A$ $j \in V - (A \cup B)$

If you think about the category of edges in $\delta(A \cap B)$, it will be one of follows.
1. $i \in A \cap B$ $j \in V - (A \cup B)$
2. $i \in A \cap B$ $j \in B - A$
3. $i \in A \cap B$ $j \in A - B$

Therefore, $z(\delta(A \cup B)) + z(\delta(A \cap B))$ will be sum of $z_e$ in following 8 categories.
1. $i \in A - B$ $j \in V - (A \cup B)$
2. $i \in A \cap B$ $j \in V - (A \cup B)$
3. $i \in B - A$ $j \in V - (A \cup B)$
4. $i \in A \cap B$ $j \in V - (A \cup B)$
5. $i \in A \cap B$ $j \in B - A$
6. $i \in A \cap B$ $j \in A - B$

Now, we can do a mapping this 6 categories to one of 8 categories for $z(\delta(A)) + z(\delta(B))$.
$1 \rightarrow 1, 2 \rightarrow 2, 3 \rightarrow 5, 4 \rightarrow 6, 5 \rightarrow 4, 6 \rightarrow 8.$
Now we have category 3, 7 lefts.
As a result, $z(\delta(A)) + z(\delta(B)) \ge z(\delta(A \cup B)) + z(\delta(A \cap B))$.

Like above, we can do the same thing for $z(\delta(A)) + z(\delta(B)) \ge z(\delta(A - B)) + z(\delta(B - A))$.

If you think about the category of edges in $\delta(A - B)$, it will be one of follows.
1. $i \in A - B$ $j \in V - (A \cup B)$
2. $i \in A - B$ $j \in A \cap B$
3. $i \in A - B$ $j \in B - A$

If you think about the category of edges in $\delta(B - A)$, it will be one of follows.
1. $i \in B - A$ $j \in V - (A \cup B)$
2. $i \in B - A$ $j \in A \cap B$
3. $i \in B - A$ $j \in A - B$

Therefore, $z(\delta(A)) + z(\delta(B)) \ge z(\delta(A - B)) + z(\delta(B - A))$ will be sum of $z_e$ in following 8 categories.
1. $i \in A - B$ $j \in V - (A \cup B)$
2. $i \in A - B$ $j \in A \cap B$ = $i \in A \cap B$ $j \in A - B$
3. $i \in A - B$ $j \in B - A$
4. $i \in B - A$ $j \in V - (A \cup B)$
5. $i \in B - A$ $j \in A \cap B$ = $i \in A \cap B$ $j \in B - A$
6. $i \in B - A$ $j \in A - B$

We can map $1 \rightarrow 1, 2 \rightarrow 8, 3 \rightarrow 3, 4 \rightarrow 5, 5 \rightarrow 4, 6 \rightarrow 7$.
Now we have category 2, 6 lefts.
As a result, $z(\delta(A)) + z(\delta(B)) \ge z(\delta(A - B)) + z(\delta(B - A))$.

Now, let $z_e = 1$ if $e \in F$ and $z_e = 0$ otherwise.

Then, $f_i(S) = f(S) - |\delta(S) \cap F| = f(S) - z(\delta(S))$.
As a result $f_i(S)$ should fulfill one of follows.
1. $f_i(A) + f_i(B) = f(A) + f(B) - z(\delta(A)) - z(\delta(B)) \le f(A \cup B) + f(A \cap B) - z(\delta(A \cup B)) - z(\delta(A \cap B)) = f_i(A \cup B) + f_i(A \cap B)$.
2. $f_i(A) + f_i(B) = f(A) + f(B) - z(\delta(A)) - z(\delta(B)) \le f(A - B) + f(B - A) - z(\delta(A - B)) - z(\delta(B - A)) = f_i(A - B) + f_i(B - A)$.

Now, we showed that $f_i$ is a weakly supermodular.

Therefore, we can found at least one edge such that $x_e \ge \frac{1}{2}$ for any $f_i$.
Now if we can show that "Minimize $\sum\limits_{e \in E - F} c_e x_e$ such that $\sum\limits_{e \in \delta(S), e \in E - F} x_e \ge f_i(S) = f(S) - |\delta(S) \cap F|$ $\forall S \subset V$, $0 \le x_e \le 1$" can be solved in polynomial time, it will the end of the proof for polynomial execution time for the algorithm.

We still can use network flow algorithm to solve this.
Therefore, we will make a seperation orcale like below.
1. Make a graph that consists of $G = (V,E)$ and set capacity of $E - F$ as $x_e$ and capacity of $F$ as 1.
2. Solve the network flow problem between all pair $i$ and $j$ such that $i \neq j$ and $i,j \in V$.
3. If there is a flow such that the value of flow $f_{ij}$ is less than $r_{ij}$ then it's an infeasible solution otherwise it is feasible.

Notice that if there is a flow then it means $\sum\limits_{e \in \delta(S)} f_e + |\delta(S) \cap F| \ge r_{ij}$ which $\delta(S)$ denotes the set of edges in $E - F$ between $S$ and $V - S$.
Which means $\sum\limits_{e \in \delta(S)} f_e \ge r_{ij} - |\delta(S) \cap F|$.
If there is some $i, j$ such that flow between $i$ and $j$ is less than $r_{ij}$ then there should be $S$ such that $\sum\limits_{e \in \delta(S)} f_e + |\delta(S) \cap F|< r_{ij}$.
As a result, we can use this as a seperation orcale.

Now only left is to show that solution is in $2\operatorname{OPT}$.
We will prove this as an inductive way.
If we recap the problem "Minimize $\sum\limits_{e \in E - F} c_e x_e$ such that $\sum\limits_{e \in \delta(S), e \in E - F} x_e \ge f_i(S) = f(S) - |\delta(S) \cap F|$", we can know that it will give a solution in $2\operatorname{OPT}$ because solution of above LP $\opertorname{ANS}$ will better than $\operatorname{OPT}$.
Also, we will select every $x_e \ge \frac{1}{2}$ to $1$.
If we define rounded solution as $\opertorname{RND}$ then $\opertorname{RND} \le 2\opertorname{ANS} \le 2\opertorname{OPT}$.

Now, let's define $\Tau_i = \cup\limits_{k = 1}^{i} F_i$ and solution of LP in $i$th iteration as $x_e^i$.
Then let's assume that algorithm terminates in $m$th iteration and let $\Tau = \Tau_m$.

Now, we need some facts to prove this inductive method.
Let assume that statement hold for $i$ iterations.
Then, $\sum\limits_{e \in \Tau_i}c_e \le 2\operatorname{OPT}$ can be assumed to be true.
At the $i + 1$ iteration, $\sum\limits_{e \in E - \Tau_i} c_e x^{i+1}_e$ $\le$ $\sum\limits_{e \in E - \Tau_i} c_e x^i_e$ is true because $x^{i+1}_e$ is the solution of "Minimize $\sum\limits_{e \in E - \Tau_i} c_e x_e$ such that $\sum\limits_{e \in \delta(S), e \in E - \Tau_i} x_e \ge f_i(S)$".

From facts above, $\sum\limits_{e \in \Tau_{i + 1}}c_e$ $=$ 
$\sum\limits_{e \in \Tau_{i + 1} - F_{i + 1}}c_e + \sum\limits_{e \in F_{i + 1}}c_e$ $\le$
$2\sum\limits_{e \in \Tau _{i + 1}- F_{i + 1}}c_e x^{i + 1}_e + 2\sum\limits_{e \in F_{i + 1}}c_e x^i_e$ $\le$

{: .box-note}
**Reference** David P. Williamson and David B. Shmoys, The Design of Approximation Algorithms.