---
layout: post
title: Approximation of metrics by tree metrics
gh-repo: daattali/beautiful-jekyll
tags: [algorithm, approximation]
comments: true
use_math: true
---

For a given vertices $V$ and distance $V \times V \rightarrow \mathcal{R} : d$.
$(V,d)$ is so called a metric if following properties are hold.

1. $d_{uv}$ $\ge$ $0$ for all $u,v$ $\in$ $V$
2. $d_{uv}$ $=$ $0$ iff $u = v$
3. $d_{uv}$ $=$ $d_{vu}$ for all $u,v$ $\in$ $V$
4. (Triangular inequality) $d_{uv}$ $\le$ $d_{uw}$ $+$ $d_{wv}$ for all $u,v,w$ $\in$ $V$

Notice that we say it is a pseudometric if property 2 changes to "$d_{uv}$ $=$ $0$ if $u = v$".

A tree metic $(V', T)$ for a set of vertices $V$ is a tree $T$ defined on a set of nodes $V' \supseteq V$, whoese edges are given non negative lengths.
For $u,v$ $\in$ $V'$, let $T_{uv}$ denote the length of the unique path between $u$ and $v$ in $T$.

Notice that tree matrix is a matric.
It is trivial to be hold for 1 ~ 3.
For 4, if there is a path $u$ to $w$ and $w$ to $v$, then concatinating them is a path from $u$ to $v$.
It will have the length equal or less than sum of each path because there could be a cycle and it can be removed.

Given a metric $d$ on $V$, we say a tree metric $(V', T)$ is a $\operatorname{tree metic embedding}$ of distortion $\alpha$ if $d_{uv}$ $\le$ $T_{uv}$ $\le$ $\alpha d_{uv}$ for all $u,v$ $\in$ $V$.

However, do we even have any approximation for it in every time?
Yes, if we have gigantic distortion.
However, it's no with some distortion.
In fact it is known that there is no tree metric has distortion less than $\frac{n - 1}{8}$ for some metric $d$ on $n$ vertices.
However there is a good theorem.

Given a metric $d$ on $V$ such that $d_{uv}$ $\ge$ $1$ for all $u$ $\neq$ $v$, there exists a randomized algorithm that finds a tree metric $(V', T)$ such that for all $u, v$ $\in$ $V$, $d_{uv}$ $\le$ $T_{uv}$ and $E[T_{uv}]$ $\le$ $O(\ln \left\vert V \right\vert)d_{uv}$.
Notice that this randomized algorithm picks a tree metric from a given graph.
Proof is like follow.

Consider the following algorithm where $B(x, r)$ is a hypersphere with the center at $x$ and radius of $r$.
<div class="alg">
    Pick $r_0$ $\in$ $[\frac{1}{2}, 1)$ uniformly at random<br>
    Choose $\Delta$ as the smallest power of two greater or equal than $2\max_{u,v \in V}d_{uv}$<br>
    Let $r_i$ $=$ $2^ir_0$ for $1$ $\le$ $i$ $\le$ $\log_2 \Delta$<br>
    Pick a permutation $\pi$ of $v$ uniformly at random<br>
    $\mathcal{C}(\log_2 \Delta) \leftarrow \{V\}$<br>
    Create a node corresponding to $V$ and make it the root node<br>
    $\operatorname{for}$ $i \leftarrow \log_2 \Delta, \log_2 \Delta - 1, \cdots, 1$
    <div class="alg">
        $\mathcal{C}(i - 1) \leftarrow \emptyset$<br>
        $\operatorname{for}$ $C \in \mathcal{C}(i)$
        <div class="alg">
            $S \leftarrow C$<br>
            $\operatorname{for}$ $j \leftarrow 1, 2, \cdots, \left\vert V \right\vert$<br>
            <div class="alg">
                $\operatorname{if}$ $B(\pi(j), r_{i-1})$ $\cap$ $S$ $\neq$ $\emptyset$<br>
                <div class="alg">
                    Add $\{B(\pi(j), r_{i-1}) \cap S\}$ to $\mathcal{C}(i -1)$<br>
                    $S$ $\leftarrow$ $S$ $-$ $(B(\pi(j), r_{i-1}) \cap S)$
                </div>
            </div>
            Create nodes corresponding to each set in $\mathcal{C}(i -1)$ and attach each node to the node in $\mathcal{C}(i)$ corresponding to its superset by an edge of length $2^i$
        </div> 
    </div>
    $V' \leftarrow$ all nodes in $\bigcup\limits_{k = 0}^{\log_2 \Delta}\mathcal{C}(k)$<br>
    $T \leftarrow$ all edges between $\mathcal{C}(k)$ and $\mathcal{C}(k - 1)$ for all $1$ $\le$ $k$ $\le$ $\log_2 \Delta$<br>
    return $(V', T)$ 
</div>

For an example, following graph's result will be like follow.

If given metric is like follow.<br>
<canvas id="canvas1" width="200" height="200" style="border:1px solid #d3d3d3;">
    Your browser does not support the HTML canvas tag.</canvas><br>
Returned tree metric can be like follow.<br>
<canvas id="canvas2" width="200" height="200" style="border:1px solid #d3d3d3;">
    Your browser does not support the HTML canvas tag.</canvas><br>
<script language = "javascript">
    let c = document.getElementById("canvas1");
    let ctx = c.getContext("2d");
    ctx.fillStyle = "white";
    ctx.beginPath();
    ctx.arc(100, 100, 80, 0, 2*Math.PI);
    ctx.stroke();
    ctx.beginPath();
    ctx.arc(100, 180, 10, 0, 2*Math.PI);
    ctx.stroke();
    ctx.fill();
    ctx.beginPath();
    ctx.arc(100, 20, 10, 0, 2*Math.PI);
    ctx.stroke();
    ctx.fill();
    ctx.beginPath();
    ctx.arc(20, 100, 10, 0, 2*Math.PI);
    ctx.stroke();
    ctx.fill();
    ctx.beginPath();
    ctx.arc(180, 100, 10, 0, 2*Math.PI);
    ctx.stroke();
    ctx.fill();
    ctx.textAlign = "center";
    ctx.fillStyle = "red";
    ctx.font = "20px Arial";
    ctx.fillText('A', 100, 180);
    ctx.fillText('B', 100, 20);
    ctx.fillText('C', 20, 100);
    ctx.fillText('D', 180, 100);
    ctx.fillText('1', 44, 44);
    ctx.fillText('1', 156, 44);
    ctx.fillText('1', 44, 156);
    ctx.fillText('1', 156, 156);
    c = document.getElementById("canvas2");
    ctx = c.getContext("2d");
  	ctx.beginPath();
    ctx.fillStyle = "black";
  	ctx.moveTo(175, 170);
  	ctx.lineTo(125, 110);
  	ctx.lineTo(100, 40);
  	ctx.lineTo(75, 110);
  	ctx.lineTo(25, 170);
  	ctx.moveTo(75, 110);
  	ctx.lineTo(75, 170);
  	ctx.moveTo(75, 110);
  	ctx.lineTo(125, 170);
    ctx.stroke();
    ctx.fillStyle = "white";
    ctx.beginPath();
    ctx.arc(25, 170, 20, 0, 2*Math.PI);
    ctx.stroke();
    ctx.fill();
    ctx.beginPath();
    ctx.arc(75, 170, 20, 0, 2*Math.PI);
    ctx.stroke();
    ctx.fill();
    ctx.beginPath();
    ctx.arc(125, 170, 20, 0, 2*Math.PI);
    ctx.stroke();
    ctx.fill();
    ctx.beginPath();
    ctx.arc(175, 170, 20, 0, 2*Math.PI);
    ctx.stroke();
    ctx.fill();
    ctx.beginPath();
    ctx.arc(75, 110, 20, 0, 2*Math.PI);
    ctx.stroke();
    ctx.fill();
    ctx.beginPath();
    ctx.arc(125, 110, 20, 0, 2*Math.PI);
    ctx.stroke();
    ctx.fill();
    ctx.beginPath();
    ctx.arc(100, 40, 20, 0, 2*Math.PI);
    ctx.stroke();
    ctx.fill();
    ctx.textAlign = "center";
    ctx.fillStyle = "red";
    ctx.font = "15px Arial";
    ctx.fillText('4', 80, 80);
    ctx.fillText('4', 120, 80);
    ctx.fillText('2', 160, 140);
    ctx.fillText('2', 110, 140);
    ctx.fillText('2', 65, 145);
    ctx.fillText('2', 45, 140);
    ctx.fillText('{A,B,C,D}', 100, 40);
    ctx.fillText('{A,B,C}', 75, 110);
    ctx.fillText('{D}', 125, 110);
    ctx.fillText('{A}', 25, 170);
    ctx.fillText('{B}', 75, 170);
    ctx.fillText('{C}', 125, 170);
    ctx.fillText('{D}', 175, 170);
</script>

Now, following is true if we call the deepest nodes as level 0 and level $i$'s parent as level $i + 1$.
Note that each node at level 0 corresponding to a singleton and every vertex in $V$ appears exactly once.
The reason is that there can't be more than center itself because $r_0$ $<$ $1$ $\le$ $d_{uv}$ for all $u,v$ $\in$ $V$.
Notice that every vertex at level $i$ is belongs to a hyper sphere of radius $r_i$ and centered by one of vertex in side of the set.
Which also means that level $\log_2 \Delta$ will contain entire $V$ because $r_{\log_2 \Delta}$ $=$ $2^{\log_2 \Delta} r_0$ $\ge$ $2^{\log_2 \Delta} \frac{1}{2}$ $=$ $\Delta \frac{1}{2}$ $\ge$ $2\max_{u,v \in V}d_{uv}\frac{1}{2}$ $=$ $\max_{u,v \in V}d_{uv}$.

Now, let's denote some terminologies for a node $n$ in the $(V', T)$.
1. $\mathcal{L}_n$ is the level of the $n$. Notice that level of root node is $\log_2 \Delta$ and level of leaf node is $0$.
2. $\mathcal{S}_n$ is the set of vertices which the corresponding hyper sphere includes.

Then, there are some facts.
1. $d_{yz}$ $\le$ $2r_{\mathcal{L}_n}$ for any $y,z$ $\in$ $\mathcal{S}_n$ becuase it should be in the same hyper sphere.
2. For any $u,v$ $\in$ $V$, they can't belongs to the same node at level $[\log_2 d_{uv}] - 1$ because otherwise $d_{uv}$ $\le$ $2r_{[\log_2 d_{uv}] - 1}$ $=$ $2 \cdot 2^{[\log_2 d_{uv}] - 1}r_0$ $=$ $2^{[\log_2 d_{uv}]}r_0$ $\le$ $2^{\log_2 d_{uv}}r_0$ $=$ $d_{uv}r_0$ $<$ $d_{uv}$ which is a contradiction.

Then, $T_{uv}$ $\ge$ $d_{uv}$ is true.
Proof is like follow.
If $d_{uv}$ $<$ $4$ then, $T_{uv}$ $\ge$ $d_{uv}$ because $T_{uv}$ $\ge$ $4$.
Notice that $T_{uv}$ $\ge$ $4$ is true because all edges in the $T$ is bigger or equal than $2$ and there should be at least one parent to go $v$ from $u$.
Now, other cases are $d_{uv}$ $\ge$ $4$.
Frist, $d_{uv}$ $\le$ $\sum\limits_{k = 0}^{[\log_2 d_{uv}]} 2^k$ because RHS is bigger than a binary representation of $d_{uv}$.
Then, $d_{uv}$ $\le$ $\sum\limits_{k = 0}^{[\log_2 d_{uv}]} 2^k$ $=$ $\sum\limits_{k = 1}^{[\log_2 d_{uv}]} 2^k$ $+$ $1$ $\le$ $\sum\limits_{k = 1}^{[\log_2 d_{uv}]} 2^k$ $+$ $\sum\limits_{k = 1}^{[\log_2 d_{uv}]} 2^k$ $=$ $2\sum\limits_{k = 1}^{[\log_2 d_{uv}]} 2^k$ $\le$ $T_{uv}$.
Notice that last inequality comes from the fact 2 above.
If we think about path from $u$ to $v$, it should go to at least level $[\log_2 d_{uv}]$ because it can't belongs to the same node untill level $[\log_2 d_{uv}] - 1$.
Also, $2\sum\limits_{k = 1}^{[\log_2 d_{uv}]} 2^k$ is a sum of length from $u$ to $v$ via level $[\log_2 d_{uv}]$.
Therefore, claim holds.

Now there are only one thing to show that $E[T_{uv}]$ $\le$ $O(\ln \left\vert V \right\vert)d_{uv}$.

To show this, there some terminologies to define.

1. $\mathcal{A}_{uv}$ is the least common ancestor of $u$ and $v$.
2. We say $w$ settles the pair of $u$ and $v$ on $i$ if $w$ is the first vertex in permutation $\pi$ such that at least one of $u$ and $v$ is in the hypersphere $B(w, r_i)$.
3. We say $w$ cut $u$ and $v$ on level $i$ if exactly one of $u$ and $v$ is in the hypersphere $B(w, r_i)$.
4. $X_{iw}(u,v)$ be the event that $w$ cuts $(u, v)$ on level $i$.
5. $S_{iW}(u,v)$ be the event that $w$ settles $(u, v)$ on level $i$.
6. $\mathbb{1}(x)$ is an indicator fuction such that $1$ if $x$ is true $0$ otherwise.

First, $T_{uv}$ $=$ 
$2\sum_{k=1}^{\mathcal{L}\_{\mathcal{A}\_{uv}}}2^k$ $=$ 
$2(2^{\mathcal{L}\_{\mathcal{A}\_{uv}} + 1} - 2)$ $=$ 
$2^{\mathcal{L}\_{\mathcal{A}\_{uv}} + 2} - 4$ $\le$ 
$2^{\mathcal{L}\_{\mathcal{A}\_{uv}} + 2}$.

Then, $T_{uv}$ $\le$ $\max\limits_{i = 0}^{\log_2 \Delta - 1} \mathbb{1}(\exists X_{iw}(u,v) \cap S_{iw}(u,v) \text{ for } w \in V) 2^{i + 3}$.
Notice that $\operatorname{argmax}\limits_{i = 0}^{\log_2 \Delta - 1} \mathbb{1}(\exists X_{iw}(u,v) \cap S_{iw}(u,v) \text{ for } w \in V)2^{i + 3}$
is the $\mathcal{A}_{uv}$.
Therefore, $T_{uv}$ $\le$ $\max\limits_{i = 0}^{\log_2 \Delta - 1} \mathbb{1}(\exists X_{iw}(u,v) \cap S_{iw}(u,v) \text{ for } w \in V) 2^{i + 3}$
$\le$ $\sum\limits_{w \in V}\sum\limits_{i = 0}^{\log_2 \Delta - 1} \mathbb{1}(\exists X_{iw}(u,v) \cap S_{iw}(u,v)) 2^{i + 3}$.

{: .box-note}
**Reference** David P. Williamson and David B. Shmoys, The Design of Approximation Algorithms.