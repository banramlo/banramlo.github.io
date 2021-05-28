---
layout: post
title: Graph coloring
gh-repo: daattali/beautiful-jekyll
tags: [algorithm, approximation]
comments: true
use_math: true
---

We need some definitions and theorems to discuss about the coloring.

### Definitions
1. A coloring of graph $G$ is an assignment of colors to the vertices of $G$.
In this case, it can be considered as the assignemnts of sets like vertex 1 to set 1 and vetex 2 to set 3 and so-on.
2. A graph is is $k$-coloring if each vertex is assigned to exactly one of $k$ colors.
3. A coloring is proper if no two adjacent vertices are assigned to the same color.
4. A graph is so-called $k$-colorable if it has a proper $k$-coloring.
5. We say $g(n)$ $=$ $\tilde{O}(f(n))$ if there exists some constant $c$ $\ge$ $0$ such that $g(n)$ $=$ $O(f(n))\ln^c$.
This is some extention that ignoring logarithmic terms.
6. We say coloring is a semicoloring if at most $\frac{\left\vert V \right\vert}{4}$ edges have endpoints with the same color.
Notice that semicoloring don't need to be proper.
It allows $\frac{\left\vert V \right\vert}{4}$ edges to be collide.

### Theorems
1. There is a polynomial time algorithm to check 2-colorable.
Notice that we can do it by DFS and checking there is some pair of vertices that colors collide.
2. It is known to be NP-hard to decide if a graph is 3-colorable.
3. It is known to be NP-hard to decide if a graph is 3-colorable or any of its proper coloring requires at least five colors.
This means input will give like 3-colorable, 5-colorable, 6-colorable, etc..
There will no 4-colorable inputs.
4. It is known to be NP-hard to find a 3-coloring from a 3-colorable graph.
Notice that if there is such an algorithm in polynomial time then running that algorithm and checking the result are enough to solve 3-colorable problem.
5. A graph with maximum degree $\delta$ can be $(\delta + 1)$-colored in polynomial time.
Notice that all of vertices never run out of colors in this case because even all the neighbors use seperate colors, there are still one color to use.
6. 3-colorable graph can be $O(\sqrt{n})$-colored in polynomial time for $n$ as the number of vertices.
7. There exists a polynomial time random algorithm that finds a semicoloring with $4\Delta^{\log_3 2}$ colors at least $\frac{1}{2}$ probability where $\Delta$ is the maximum degree of a graph with.
8. There is a polynomial algorithm that finds $\tilde{O}(n^{\log_6 2})$-coloring of a given 3-colorable graph at least $\frac{1}{2}$ probability.

### Proof of theorems

Theorem 6 can be done by following algorithm.
<div class="alg">
    $\operatorname{while} \exists$ a vertex $v$ with at least degree $\sqrt{n}$<br>
    <div class="alg">
        Choose three new colors $c_1, c_2, c_3$<br>
        Color $v$ with $c_1$<br>
        Color neighbors of $v$ with $c_2, c_3$<br>
        Remove colored vertices
    </div>
    Color the rest of graph with $\sqrt{n}$ new colors
</div>
Notice that the neighbors of $v$ can be colored with 2 colors in each iteration because given graph is a 3-colorable graph.
The reason is that all of neighbor of $v$ can't be $c_1$ if we select $v$ as $c_1$.
Then, it's a 2-coloring problem which can be solve in polynomial time.
With that, $v$ and its neighbors are using distinct from other vertices because it producess new colors.
Notice that we can remove it because it uses independent color sets.
Now, it uses just $\sqrt{n}$ color at the last.
Therefore it's enough to show there are at most $O(\sqrt{n})$ iteration for " $\operatorname{while} \exists$ a vertex $v$ with at least degree $\sqrt{n}$".
With that, it removes at least $\sqrt{n}I$ vetices from graph if we denote $I$ as the number of such an iteration because each vertices has $\sqrt{n}$ degree.
However, we can't remove more vetices than number of vertices at the beginning.
Therefore, $\sqrt{n}I$ $\le$ $n$.
As a result, $I$ $\le$ $\sqrt{n}$.
Notice that algorithm uses at most $4\sqrt{n}$ colors and it runs in a polynomial time.

<br>

Theorem 7 can be done by following algorithm.

Consider the following vector program for given graph $G$ $=$ $(V,E)$.
Let $n$ $=$ $\left\vert V \right\vert$
Minimize $\lambda$<br>
such that
    $v_i \cdot v_j \le \lambda$ for all $(i, j) \in E$<br>
    $v_i \cdot v_i = 1$ for all $i \in V$<br>
    $v_i \in \mathcal{R}^n$ for all $i \in V$<br>

Then, there is a feasible solution such that $v_i \cdot v_j$ $=$ $-\frac{1}{2}$ for all $(i,j) \in E$ if given graph is a 3-colorable graph.
Proof is like follow.

Consider an equilateral triangle inscribed in the intersection of the unit hypersphere and a hyperplane containing the origin.
Fix a 3-coloring and assign one of the three vertices of the triangle as the vector of each vetex in $V$ so that vertices are assigned the same vector iff they are assigned the same color.
Notice that all $v_i$ will be on this hypersphere because "$v_i \cdot v_i = 1$ for all $i \in V$".
Now, we have $v_i \cdot v_j$ $=$ $\left\vert v_i \right\vert \left\vert v_j \right\vert \cos (\frac{2\pi}{3})$ $=$ $-\frac{1}{2}$.
Notice that angle is $\frac{2\pi}{3}$ because it's an equilateral triangle.

Then, we can design an algorithm like follow.
<div class="alg">
    Solve algorithm above to obtain optimal solution $v_1, \cdots, v_n$<br>
    Choose vector $r_1, \cdots, r_t$ independently and uniformly at random from the unit hypersphere where $t = 2 + \log_3 \Delta$.<br>
    Divides the hyper sphere into $2^t$ different regions by half space $H_i = \{x | x \cdot r_i \ge 0\}$ for $0$ $\le$ $i$ $\le$ $t$<br>
    Assign a distinct color for all vectices in each resign.
</div>

Polynomial running time of semidefinitive program/random picking will be updated later.
Then, all other process will be in the polynomial time.
Notice that this algorithm uses $2^t$ $=$ $2^{2 + \log_3 \Delta}$ $=$ $4 \times 2^{\log_3 \Delta}$ $=$ $4 {\Delta}^{\log_3 2}$ colors.
Therefore, it's enough to show that this makes a semicoloring with $\frac{1}{2}$ probability.

Now, let's think about edge $e$ $=$ $(v_i, v_j)$.
With this, consider some $H_k$ and $r^{\star}_k$.
Which $r^{\star}_k$ is the projection of $r_k$ on to the plane defined by $v_i, v_j, 0$.
Then, $v_i \cdot r_k$ $=$ $v_i \cdot r^{\star}_k$ and $v_i \cdot r_k$ $=$ $v_i \cdot r^{\star}_k$.
Notice that $r_k$ can be decomposed to $r^{\star}_k$ and $r^{\circ}_k$.
Then, $v_i \cdot r_k$ $=$ 
$v_i \cdot (r^{\star}_k + r^{\circ}_k)$ $=$ 
$v_i \cdot r^{\star}_k$ $+$ $v_i \cdot r^{\circ}_k$ $=$
$v_i \cdot r^{\star}_k$.
This works in the same way for $v_j$ either.
Now, $Pr[\text{Both } v_i \text{ and } v_j \text{ are in the } H_k]$ $=$
$Pr[\text{Both } v_i \cdot r_k \text{ and } v_j \cdot r_k \text{ has the same sign}]$ $=$ 
$Pr[\text{Both } v_i \cdot r^{\star}_k \text{ and } v_j \cdot r^{\star}_k \text{ has the same sign}]$

Then, $Pr[\text{Both } v_i \cdot r^{\star}_k \text{ and } v_j \cdot r^{\star}_k \text{ has the same sign}]$ $=$ $\frac{\pi - \theta}{\pi}$ which $\theta$ is the angle between $v_i$ and $v_j$.
Proof is like follow.
Let's denote $\theta_i, \theta_j, \theta_k$ as the angle of $v_i, v_j, r^{\star}_k$.
Then, there are two cases for $\theta_k$ which both $v_i \cdot r^{\star}_k$ and $v_j \cdot r^{\star}_k$ are positive or negative.
With this two cases, there are 2 cases per each case for each of them from criteria "$\theta_i = \theta_j + \pi$" to choose the range of possible angle.
Therefore, there are 4 cases in total.

1. $\theta_j - \frac{\pi}{2}$ $\le$ $\theta_k$ $\le$ $\theta_i + \frac{\pi}{2}$ if $\theta_i$ $\le$ $\theta_j$ $\le$ $\theta_i$ $+$ $\pi$ and both $v_i \cdot r^{\star}_k$ and $v_j \cdot r^{\star}_k$ are positive.
2. $\theta_j + \frac{\pi}{2}$ $\le$ $\theta_k$ $\le$ $\theta_i + \frac{3\pi}{2}$ if $\theta_i$ $\le$ $\theta_j$ $\le$ $\theta_i$ $+$ $\pi$ and both $v_i \cdot r^{\star}_k$ and $v_j \cdot r^{\star}_k$ are negative.
3. $\theta_i - \frac{\pi}{2}$ $\le$ $\theta_k$ $\le$ $\theta_j + \frac{\pi}{2}$ if $\theta_i$ $-$ $\pi$ $\le$ $\theta_j$ $\le$ $\theta_i$ and both $v_i \cdot r^{\star}_k$ and $v_j \cdot r^{\star}_k$ are positive.
4. $\theta_i + \frac{\pi}{2}$ $\le$ $\theta_k$ $\le$ $\theta_j + \frac{3\pi}{2}$ if $\theta_i$ $-$ $\pi$ $\le$ $\theta_j$ $\le$ $\theta_i$ and both $v_i \cdot r^{\star}_k$ and $v_j \cdot r^{\star}_k$ are negative.

Then, we can know claim holds in each cases.

1. $2\pi Pr[\text{Both } v_i \cdot r^{\star}_k \text{ and } v_j \cdot r^{\star}_k \text{ has the same sign}]$ $=$
$(\theta_i + \frac{\pi}{2})$ $-$ $(\theta_j - \frac{\pi}{2})$ $+$ $(\theta_i + \frac{3\pi}{2})$ - $(\theta_j + \frac{\pi}{2})$  $=$
$2(\theta_i - \theta_j)$ $+$ $2\pi$ $=$
$2(\pi + \theta_i - \theta_j)$ $=$
$2(\pi - (\theta_j - \theta_i))$
if $\theta_i$ $\le$ $\theta_j$ $\le$ $\theta_i$ $+$ $\pi$.
As a result, $Pr[\text{Both } v_i \cdot r^{\star}_k \text{ and } v_j \cdot r^{\star}_k \text{ has the same sign}]$ $=$ 
$\frac{\pi - (\theta_j - \theta_i)}{\pi}$.
Therefore claim holds in this case.
2. $2\pi Pr[\text{Both } v_i \cdot r^{\star}_k \text{ and } v_j \cdot r^{\star}_k \text{ has the same sign}]$ $=$
$(\theta_j + \frac{\pi}{2})$ $-$ $(\theta_i - \frac{\pi}{2})$ $+$ $(\theta_j + \frac{3\pi}{2})$ $-$ $(\theta_i + \frac{\pi}{2})$ $=$
$2(\theta_j - \theta_i)$ $+$ $2\pi$ $=$
$2(\pi + \theta_j - \theta_i)$ $=$
$2(\pi - (\theta_i - \theta_j))$
if $\theta_i$ $-$ $\pi$ $\le$ $\theta_j$ $\le$ $\theta_i$.
As a result, $Pr[\text{Both } v_i \cdot r^{\star}_k \text{ and } v_j \cdot r^{\star}_k \text{ has the same sign}]$ $=$ 
$\frac{\pi - (\theta_i - \theta_j)}{\pi}$.
Therefore claim holds in this case.

Notice that $Pr[\text{Both } v_i \text{ and } v_j \text{ are in the } H_k]$ $=$
$\frac{\pi - \theta}{\pi}$ $=$
$1$ $-$ $\frac{\theta}{\pi}$ $=$
$1$ $-$ $\frac{\arccos(v_i \cdot v_j)}{\pi}$ $=$
$1$ $-$ $\frac{1}{\pi}\arccos(v_i \cdot v_j)$.

Therefore, $Pr[\text{Both } i \text{ and } j \text{ assigned to the same color}]$ $=$ 
$Pr[\text{Both } v_i \text{ and } v_j \text{ are in the same reigon for all half space}]$ $=$ 
$(1 - \frac{1}{\pi}\arccos(v_i \cdot v_j))^t$.
With above, there are two facts from semidefinitive program and proof above.
1. $v_i \cdot v_j \le \lambda$ for all $(i, j) \in E$
2. There is a feasible solution such that $v_i \cdot v_j$ $=$ $-\frac{1}{2}$ for all $(i,j) \in E$ 

Now let's denote semidefinitive program optimum's $\lambda$ as ${\lambda}^{\star}$.
Then, ${\lambda}^{\star}$ $\le$ $-\frac{1}{2}$ because semidefinitive program was minimization problem.

Therefore, $Pr[\text{Both } i \text{ and } j \text{ assigned to the same color}]$ $=$
$(1 - \frac{1}{\pi}\arccos(v_i \cdot v_j))^t$ $\le$
$(1 - \frac{1}{\pi}\arccos({\lambda}^{\star}))^t$ $\le$
$(1 - \frac{1}{\pi}\arccos(-\frac{1}{2}))^t$ $=$
$(1 - \frac{1}{\pi}\frac{2\pi}{3})^t$ $=$
$(1 - \frac{2}{3})^t$ $=$
$(\frac{1}{3})^{2 + \log_3 \Delta}$ $=$
$\frac{1}{9\Delta}$.

Notice that $\arccos x$ is a monotonic decreasing function for $-1$ $\le$ $x$ $\le$ $1$.
Therefore, $1$ $-$ $\frac{1}{\pi}\arccos x$ is a monotonic increasing function.

Now, sum of degree is $2\left\vert E \right\vert$ and possible maximum sum of degree is $n\Delta$.
Therefore, $2\left\vert E \right\vert$ $\le$ $n\Delta$ which means $\frac{\left\vert E \right\vert}{\Delta}$ $\le$ $\frac{n}{2}$.
As a result, expected number of edges whose endpoints are colred the same is at most $\left\vert E \right\vert \frac{1}{9\Delta}$ $\=$ $\frac{\left\vert E \right\vert}{9\Delta}$ $\le$ $\frac{n}{18}$.

Then, $Pr[X \ge \frac{n}{4}]$ $\le$ $\frac{E[X]}{n/4}$ $=$ $\frac{4}{n}E[X]$ $\le$ $\frac{4}{n}\frac{n}{18}$ $=$ $\frac{2}{9}$ $\le$ $\frac{1}{2}$ with markov's inequality where $X$ denotes number of edges where endpoiunts are colored the same.
Therefore claim holds.

<br>

Theorem 8 can be done by following algorithm.
Let $\sigma = n^{\log_6 3}$.
<div class="alg">
    Let's call the below part as "part 1"
    <div class="alg">
        $\operatorname{while} \exists$ a vertex $v$ with degree $\ge$ $\sigma$<br>
        <div class="alg">
            Color $v$ and it's neighbors using three new colors<br>
            Remove the colored vertices
        </div>
    </div>
    Let's call the below part as "part 2"
    <div class="alg">
        $\operatorname{while}$ the graph is not empty, repeat at most $\log_2 n$ times<br>
        <div class="alg">
           Try running Algorithm at theorem 7 with $[\log_2(\log_2 n)] + 1$ times to find a semicoloring<br>
           $R \leftarrow$ endpoints of edges whose endpoints are colored the same<br>
           Color $V - R$ according to the semicoloring, using new colors<br>
           Remove the colored vetices
        </div>
    </div>
    Color the remaining vertices with distinct new colors
</div>

Now, it is trivial that this algorithm runs in a polynomial time.
Therefore, it is enough to show it returns $\tilde{O}(n^{\log_6 2})$-coloring at least $\frac{1}{2}$ probability.
Now, let's think about part 1.
Then, maximum number of iteration is at most $[\frac{n}{\sigma}] + 1$ because it remvoes at least $\sigma$ vertices at once.
Therefore, color used at part 1 is at most $3([\frac{n}{\sigma}] + 1)$.
Now, let's think about part 2.
Left part of graph has at most $\sigma$ degree because we removed every possible vertices with more than degree $\sigma$.
Now, let's denote the maximum of degree after part 1 as $\Delta$ and the number of vertices as $n'$.
If we see each iteration, it fails to find a semicoloring with $(\frac{1}{2})^{[\log_2(\log_2 n)] + 1}$ probability.
Which is $(\frac{1}{2})^{[\log_2(\log_2 n)] + 1}$ $=$
$(2)^{-[\log_2(\log_2 n)] - 1}$ $\le$
$(2)^{-\log_2(\log_2 n) - 1}$ $=$
$(2)^{-\log_2(\log_2 n)}\frac{1}{2}$ $=$
$(2)^{\log_2(\log_2 n)^{-1}}\frac{1}{2}$ $=$
$(\log_2 n)^{-1}\frac{1}{2}$ $=$
$\frac{1}{2\log_2 n}$.
Therefore, it fails at most $\frac{1}{2\log_2 n}$ probability.
As a result, this algorithm success to find a semicoloring in every iteration at least $(1 - \frac{1}{2\log_2 n})^{\log_2 n}$ $\ge$ $\frac{1}{2}$ probability if $n$ $\ge$ $2$.

Proof is like follow.
Notice that if we change $log_2 x$ to $t$ then $(1 - \frac{1}{2\log_2 n})^{\log_2 n}$ $=$ $(1 - \frac{1}{2t})^{t}$.
Now it can be derivated easily about $t$ and result will be $(1 - \frac{1}{2t})(\ln(1 - \frac{1}{2t}) + \frac{1}{2t - 1})$.
Notice that $t \ge 1$.
Then, $1 - \frac{1}{2t} \ge 0$ because $t \ge 1$.
For the other part, $\ln(1 - \frac{1}{2t})$ $+$ $\frac{1}{2t - 1}$ $=$ 
$\ln(\frac{2t - 1}{2t})$ $+$ $\frac{1}{2t - 1}$ $=$ 
$\ln(\frac{2t - 1}{2t})$ $+$ $\frac{2t}{2t - 1}$ $-$ $\frac{2t - 1}{2t - 1}$ $=$
$\ln(\frac{2t - 1}{2t})$ $+$ $\frac{2t}{2t - 1}$ $-$ $1$ $=$
$\ln X$ $+$ $\frac{1}{X}$ $-$ $1$.
for $X$ $=$ $\frac{2t -1}{2t}$.
Notice that $0$ $<$ $X$ $<$ $1$.
Then, $\ln X$ $+$ $\frac{1}{X}$ $-$ $1$ $\ge$ $0$.
Proof is like follow.
If we derivated $y$ $=$ $\ln X$ $+$ $\frac{1}{X}$ $-$ $1$, $y'$ $=$ $\frac{1}{X}$ $-$ $\frac{1}{X^2}$ $=$ $\frac{X - 1}{X^2}$.
As a result, $y$ decreases in $0$ $<$ $X$ $<$ $1$.
However, it's zero when $X$ $=$ $1$.
Therefore, $\ln X$ $+$ $\frac{1}{X}$ $-$ $1$ $\ge$ $0$.
As a result, this algorithm success to find a semicoloring in every iteration at least $\frac{1}{2}$ probability if $n$ $\ge$ $2$.

Now, it doesn't have too much meaning if you think about $n$ $=$ $1$.
Therefore, assumption isn't too tight.

Now, let's see how many vertices are removed at each iteration.
If we start with $n'$ vertices, $\left\vert R \right\vert$ $\le$ $2 \frac{n'}{4}$ $=$ $\frac{n'}{2}$.
As a result, it will reduce to be half at least.
Therefore, there will be at most $O(1)$ vertices after part 2.
Notice that we will do part 2 at most $log_2 n$ time and it will be enough to be so.

As a result color used in each part is like follow.
1. Part 1 uses at most $3([\frac{n}{\sigma}] + 1)$ $=$
$3([\frac{n}{n^{\log_6 3}}] + 1)$ $\le$
$3(\frac{n}{n^{\log_6 3}} + 1)$ $=$
$3(\frac{n^{\log_6 6}}{n^{\log_6 3}} + 1)$ $=$
$3(n^{\log_6 2} + 1)$ colors.
2. Part 2 uses at most $4\Delta^{\log_3 2}(\log_2 n)$ $\le$
$4\sigma^{\log_3 2}(\log_2 n)$ $=$
$4(n^{\log_6 3})^{\log_3 2}(\log_2 n)$ $=$
$4n^{\log_6 3 \cdot \log_3 2}(\log_2 n)$ $=$
$4n^{\log_6 2}(\log_2 n)$ colors.
3. Left part will use at most $O(1)$ colors.

As a result, algorithm uses at most $3n^{\log_6 2}$ $+$ $3$ $+$ $4n^{\log_6 2}(\log_2 n)$ $+$ $O(1)$.
Therefore, claim holds.

{: .box-note}
**Reference** David P. Williamson and David B. Shmoys, The Design of Approximation Algorithms.