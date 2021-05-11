---
layout: post
title: LP Duality
gh-repo: daattali/beautiful-jekyll
tags: [algorithm, approximation]
comments: true
use_math: true
---

For any given LP, we can construct LP like one of the format follow.

### Minimization problem
For given vectors
$X = \begin{pmatrix} x_1 \\\ x_2 \\\ \vdots \\\ x_n \end{pmatrix}$, 
$C = \begin{pmatrix} c_1 \\\ c_2 \\\ \vdots \\\ c_n \end{pmatrix}$,
$B = \begin{pmatrix} b_1 \\\ b_2 \\\ \vdots \\\ b_m \end{pmatrix}$ and matrix 
$A = \begin{pmatrix} a_{11} & a_{12} & \cdots & a_{1n} \\\ a_{21} & a_{22} & \cdots & a_{2n} \\\ \vdots & \vdots & \ddots & \vdots \\\ a_{m1} & a_{m2} & \cdots & a_{mn} \end{pmatrix} = \begin{pmatrix} a_1 \\\ a_2 \\\ \vdots \\\ a_m \end{pmatrix}$.

Minimize $C^T X$ such that <br>
    $a_i X \ge b_i$ for $i \in C_p$,<br>
    $a_i X \le b_i$ for $i \in C_m$,<br>
    $a_i X = b_i$ for $i \in C_e$,<br>
    $x_i \ge 0$ for $i \in B_p$,<br>
    $x_i \le 0$ for $i \in B_m$,<br>
    No limitation $x_i$ for $i \in B_f$.

Notice that $C_p \cup C_m \cup C_e = \\{1, 2, \cdots, m\\}$, $C_p \cap C_m$ $=$ $C_m \cap C_e$ $=$ $C_p \cap C_e$ $=$ $\emptyset$, $B_p \cup B_m \cup B_f = \\{1, 2, \cdots, n\\}$, $B_p \cap B_m$ $=$ $B_m \cap B_f$ $=$ $B_p \cap B_f$ $=$ $\emptyset$

### Maximization problem
For given vectors
$X = \begin{pmatrix} x_1 \\\ x_2 \\\ \vdots \\\ x_n \end{pmatrix}$, 
$C = \begin{pmatrix} c_1 \\\ c_2 \\\ \vdots \\\ c_n \end{pmatrix}$,
$B = \begin{pmatrix} b_1 \\\ b_2 \\\ \vdots \\\ b_m \end{pmatrix}$ and matrix 
$A = \begin{pmatrix} a_{11} & a_{12} & \cdots & a_{1n} \\\ a_{21} & a_{22} & \cdots & a_{2n} \\\ \vdots & \vdots & \ddots & \vdots \\\ a_{m1} & a_{m2} & \cdots & a_{mn} \end{pmatrix} = \begin{pmatrix} a_1 \\\ a_2 \\\ \vdots \\\ a_m \end{pmatrix}$.

Maximize $C^T X$ such that <br>
    $a_i X \ge b_i$ for $i \in C_p$,<br>
    $a_i X \le b_i$ for $i \in C_m$,<br>
    $a_i X = b_i$ for $i \in C_e$,<br>
    $x_i \ge 0$ for $i \in B_p$,<br>
    $x_i \le 0$ for $i \in B_m$,<br>
    No limitation $x_i$ for $i \in B_f$.

Notice that $C_p \cup C_m \cup C_e = \\{1, 2, \cdots, m\\}$, $C_p \cap C_m$ $=$ $C_m \cap C_e$ $=$ $C_p \cap C_e$ $=$ $\emptyset$, $B_p \cup B_m \cup B_f = \\{1, 2, \cdots, n\\}$, $B_p \cap B_m$ $=$ $B_m \cap B_f$ $=$ $B_p \cap B_f$ $=$ $\emptyset$

### Duality
For any given problem, it is so-called dual of primal problem if it fulfills following condition. 
Let's assume that primal is a minimization problem like follow.

For given vectors
$X = \begin{pmatrix} x_1 \\\ x_2 \\\ \vdots \\\ x_n \end{pmatrix}$, 
$C = \begin{pmatrix} c_1 \\\ c_2 \\\ \vdots \\\ c_n \end{pmatrix}$,
$B = \begin{pmatrix} b_1 \\\ b_2 \\\ \vdots \\\ b_m \end{pmatrix}$ and matrix 
$A = \begin{pmatrix} a_{11} & a_{12} & \cdots & a_{1n} \\\ a_{21} & a_{22} & \cdots & a_{2n} \\\ \vdots & \vdots & \ddots & \vdots \\\ a_{m1} & a_{m2} & \cdots & a_{mn} \end{pmatrix} = \begin{pmatrix} a_1 \\\ a_2 \\\ \vdots \\\ a_m \end{pmatrix}$.

Minimize $C^T X$ such that <br>
    $a_i X \ge b_i$ for $i \in C_p$,<br>
    $a_i X \le b_i$ for $i \in C_m$,<br>
    $a_i X = b_i$ for $i \in C_e$,<br>
    $x_i \ge 0$ for $i \in B_p$,<br>
    $x_i \le 0$ for $i \in B_m$,<br>
    No limitation for $x_i$ for $i \in B_f$.

Then dual of primal minimization problem is a maximization problem like follow.

For given vectors
$Y = \begin{pmatrix} y_1 \\\ y_2 \\\ \vdots \\\ y_m \end{pmatrix}$, 
$C = \begin{pmatrix} c_1 \\\ c_2 \\\ \vdots \\\ c_n \end{pmatrix}$,
$B = \begin{pmatrix} b_1 \\\ b_2 \\\ \vdots \\\ b_m \end{pmatrix}$ and matrix 
$A = \begin{pmatrix} a_{11} & a_{12} & \cdots & a_{1n} \\\ a_{21} & a_{22} & \cdots & a_{2n} \\\ \vdots & \vdots & \ddots & \vdots \\\ a_{m1} & a_{m2} & \cdots & a_{mn} \end{pmatrix} = \begin{pmatrix} A_1 & A_2 & \cdots & A_n \end{pmatrix}$.

Maximize $B^T Y$ such that <br>
    $y_i \ge 0$ for $i \in C_p$,<br>
    $y_i \le 0$ for $i \in C_m$,<br>
    No limitation for $y_i$ for $i \in C_e$,<br>
    $A_i^T Y \ge c_i$ for $i \in B_p$,<br>
    $A_i^T Y \le c_i$ for $i \in B_m$,<br>
    $A_i^T Y = c_i$ for $i \in B_f$.

It's the same for the opposite.

For given vectors
$Y = \begin{pmatrix} y_1 \\\ y_2 \\\ \vdots \\\ y_m \end{pmatrix}$, 
$C = \begin{pmatrix} c_1 \\\ c_2 \\\ \vdots \\\ c_n \end{pmatrix}$,
$B = \begin{pmatrix} b_1 \\\ b_2 \\\ \vdots \\\ b_m \end{pmatrix}$ and matrix 
$A = \begin{pmatrix} a_{11} & a_{12} & \cdots & a_{1n} \\\ a_{21} & a_{22} & \cdots & a_{2n} \\\ \vdots & \vdots & \ddots & \vdots \\\ a_{m1} & a_{m2} & \cdots & a_{mn} \end{pmatrix} = \begin{pmatrix} A_1 & A_2 & \cdots & A_n \end{pmatrix}$.

Maximize $B^T Y$ such that <br>
    $y_i \ge 0$ for $i \in C_p$,<br>
    $y_i \le 0$ for $i \in C_m$,<br>
    No limitation for $y_i$ for $i \in C_e$,<br>
    $A_i^T Y \ge c_i$ for $i \in B_p$,<br>
    $A_i^T Y \le c_i$ for $i \in B_m$,<br>
    $A_i^T Y = c_i$ for $i \in B_f$.

Then dual of primal maximization problem is a minimization problem like follow.

For given vectors
$X = \begin{pmatrix} x_1 \\\ x_2 \\\ \vdots \\\ x_n \end{pmatrix}$, 
$C = \begin{pmatrix} c_1 \\\ c_2 \\\ \vdots \\\ c_n \end{pmatrix}$,
$B = \begin{pmatrix} b_1 \\\ b_2 \\\ \vdots \\\ b_m \end{pmatrix}$ and matrix 
$A = \begin{pmatrix} a_{11} & a_{12} & \cdots & a_{1n} \\\ a_{21} & a_{22} & \cdots & a_{2n} \\\ \vdots & \vdots & \ddots & \vdots \\\ a_{m1} & a_{m2} & \cdots & a_{mn} \end{pmatrix} = \begin{pmatrix} a_1 \\\ a_2 \\\ \vdots \\\ a_m \end{pmatrix}$.

Minimize $C^T X$ such that <br>
    $a_i X \ge b_i$ for $i \in C_p$,<br>
    $a_i X \le b_i$ for $i \in C_m$,<br>
    $a_i X = b_i$ for $i \in C_e$,<br>
    $x_i \ge 0$ for $i \in B_p$,<br>
    $x_i \le 0$ for $i \in B_m$,<br>
    No limitation for $x_i$ for $i \in B_f$.

### Example
Now, we've understood what is dual of primal.
However, where does it come from?

Let's think about the following LP.
For given vectors

Minimize $-x_1 + 3x_2 + x_3$ such that <br>
    $2x_3 + x_2 \ge 6$,<br>
    $x_1 - x_2 + x_3 \ge 4$,<br>
    $x_1 - 3x_2 \le 2$,<br>
    $2 x_1 - x_2 + x_3 = 7$,<br>
    $x_1 \ge 0$,<br>
    $x_2 \le 0$,<br>
    No limitation $x_3$.

Notice that optimal solution is $(x_1, x_2, x_3) = (2,0,3)$


{: .box-note}
**Reference** David P. Williamson and David B. Shmoys, The Design of Approximation Algorithms.