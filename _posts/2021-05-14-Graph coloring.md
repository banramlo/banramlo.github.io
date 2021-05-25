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
1. A coloring of grapg $G$ is an assignment of colors to the vertices of $G$.
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
This can be done by following algorithm.

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

7. There exists a polynomial algorithm that finds a semicoloring with $4\Delta^{\log_2 3}$ colors where $\Delta$ is the maximum degree of a graph.

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
Notice that this algorithm uses $2^t$ $=$ $2^{2 + \log_3 \Delta}$ $=$ $4 2^{\log_3 \Delta}$ $=$ $4 {\Delta}^{\log_3 2}$ colors.


8. There is a polynomial algorithm that finds $\tilde{O}(n^{\log_6 2})$-coloring of a given 3-colorable graph


{: .box-note}
**Reference** David P. Williamson and David B. Shmoys, The Design of Approximation Algorithms.