---
layout: post
title: LP Duality
gh-repo: daattali/beautiful-jekyll
tags: [algorithm, LP]
comments: true
use_math: true
---

For any given LP, we can construct LP like one of the format follow.

### Minimization problem
For given vectors
$X = \begin{pmatrix} x_1 \\\ x_2 \\\ \vdots \\\ x_n \end{pmatrix}$, 
$C = \begin{pmatrix} c_1 \\\ c_2 \\\ \vdots \\\ c_n \end{pmatrix}$,
$B = \begin{pmatrix} b_1 \\\ b_2 \\\ \vdots \\\ b_m \end{pmatrix}$ and matrix 
$A = \begin{pmatrix} a_{11} & a_{12} & \cdots & a_{1n} \\\ a_{21} & a_{22} & \cdots & a_{2n} \\\ \vdots & \vdots & \ddots & \vdots \\\ a_{m1} & a_{m2} & \cdots & a_{mn} \end{pmatrix}$ $=$ $\begin{pmatrix} a_1 \\\ a_2 \\\ \vdots \\\ a_m \end{pmatrix}$.

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
$A = \begin{pmatrix} a_{11} & a_{12} & \cdots & a_{1n} \\\ a_{21} & a_{22} & \cdots & a_{2n} \\\ \vdots & \vdots & \ddots & \vdots \\\ a_{m1} & a_{m2} & \cdots & a_{mn} \end{pmatrix}$ $=$ $\begin{pmatrix} a_1 \\\ a_2 \\\ \vdots \\\ a_m \end{pmatrix}$.

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
$A = \begin{pmatrix} a_{11} & a_{12} & \cdots & a_{1n} \\\ a_{21} & a_{22} & \cdots & a_{2n} \\\ \vdots & \vdots & \ddots & \vdots \\\ a_{m1} & a_{m2} & \cdots & a_{mn} \end{pmatrix}$ $=$ $\begin{pmatrix} a_1 \\\ a_2 \\\ \vdots \\\ a_m \end{pmatrix}$.

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
$A = \begin{pmatrix} a_{11} & a_{12} & \cdots & a_{1n} \\\ a_{21} & a_{22} & \cdots & a_{2n} \\\ \vdots & \vdots & \ddots & \vdots \\\ a_{m1} & a_{m2} & \cdots & a_{mn} \end{pmatrix}$ $=$ $\begin{pmatrix} A_1 & A_2 & \cdots & A_n \end{pmatrix}$.

Maximize $B^T Y$ such that <br>
    $y_i \ge 0$ for $i \in C_p$,<br>
    $y_i \le 0$ for $i \in C_m$,<br>
    No limitation for $y_i$ for $i \in C_e$,<br>
    $A_i^T Y \le c_i$ for $i \in B_p$,<br>
    $A_i^T Y \ge c_i$ for $i \in B_m$,<br>
    $A_i^T Y = c_i$ for $i \in B_f$.

It's the same for the opposite.

For given vectors
$Y = \begin{pmatrix} y_1 \\\ y_2 \\\ \vdots \\\ y_m \end{pmatrix}$, 
$C = \begin{pmatrix} c_1 \\\ c_2 \\\ \vdots \\\ c_n \end{pmatrix}$,
$B = \begin{pmatrix} b_1 \\\ b_2 \\\ \vdots \\\ b_m \end{pmatrix}$ and matrix 
$A = \begin{pmatrix} a_{11} & a_{12} & \cdots & a_{1n} \\\ a_{21} & a_{22} & \cdots & a_{2n} \\\ \vdots & \vdots & \ddots & \vdots \\\ a_{m1} & a_{m2} & \cdots & a_{mn} \end{pmatrix}$ $=$ $\begin{pmatrix} A_1 & A_2 & \cdots & A_n \end{pmatrix}$.
 
Maximize $B^T Y$ such that <br>
    $y_i \ge 0$ for $i \in C_p$,<br>
    $y_i \le 0$ for $i \in C_m$,<br>
    No limitation for $y_i$ for $i \in C_e$,<br>
    $A_i^T Y \le c_i$ for $i \in B_p$,<br>
    $A_i^T Y \ge c_i$ for $i \in B_m$,<br>
    $A_i^T Y = c_i$ for $i \in B_f$.

Then dual of primal maximization problem is a minimization problem like follow.

For given vectors
$X = \begin{pmatrix} x_1 \\\ x_2 \\\ \vdots \\\ x_n \end{pmatrix}$, 
$C = \begin{pmatrix} c_1 \\\ c_2 \\\ \vdots \\\ c_n \end{pmatrix}$,
$B = \begin{pmatrix} b_1 \\\ b_2 \\\ \vdots \\\ b_m \end{pmatrix}$ and matrix 
$A = \begin{pmatrix} a_{11} & a_{12} & \cdots & a_{1n} \\\ a_{21} & a_{22} & \cdots & a_{2n} \\\ \vdots & \vdots & \ddots & \vdots \\\ a_{m1} & a_{m2} & \cdots & a_{mn} \end{pmatrix}$ $=$ $\begin{pmatrix} a_1 \\\ a_2 \\\ \vdots \\\ a_m \end{pmatrix}$.

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

Minimize $-x_1 + 3x_2 + x_3$ such that <br>
    $x_2 + 2x_3 \ge 6$,<br>
    $x_1 - x_2 + x_3 \ge 4$,<br>
    $x_1 - 3x_2 \le 2$,<br>
    $2x_1 - x_2 + x_3 = 7$,<br>
    $x_1 \ge 0$,<br>
    $x_2 \le 0$,<br>
    No limitation for $x_3$.

Notice that optimal solution is $1$ with $(x_1, x_2, x_3) = (2,0,3)$.

Even though we know the optimum is $1$, we may want to provide some lower bound of the problem.
In this case, we can think about sumation of constraints.
If we think about any feasible solution $X = \begin{pmatrix}x_1 \\\ x_2 \\\ x_3 \end{pmatrix}$, all of constraints should hold.
As a result all of following is true.
1. $x_2 + 2x_3$ $\ge$ $6$
2. $x_1 - x_2 + x_3$ $\ge$ $4$
3. $x_1 - 3x_2$ $\le$ $2$
4. $2x_1 - x_2 + x_3$ $=$ $7$

Now, let's define $X = \begin{pmatrix}y_1 \\\ y_2 \\\ y_3 \\\ y_4 \end{pmatrix}$ and multiply it to all of them.
Then all of following is true.
Notice that each $y_i$ has positive or negative values to match direction of inequality.

1. $(x_2 + 2x_3) y_1$ $\ge$ $6y_1$, $y_1$ $\ge$ $0$
2. $(x_1 - x_2 + x_3)y_2$ $\ge$ $4y_2$, $y_2$ $\ge$ $0$
3. $(x_1 - 3x_2)y_3$ $\ge$ $2y_3$, $y_3$ $\le$ $0$
4. $(2x_1 - x_2 + x_3)y_4$ $=$ $7y_4$, no limitation for $y_4$

Now, let's sum up all the constraints.
$(x_2 + 2x_3)y_1$ $+$ $(x_1 - x_2 + x_3)y_2$ $+$ $(x_1 - 3x_2)y_3$ $+$ $(2x_1 - x_2 + x_3)y_4$ $\ge$ $6y_1 + 4y_2 + 2y_3 + 7y_4$.
Now, just reorder the formula with the $x_1, x_2, x_3$.
Then, $(y_2 + y_3 + 2y_4)x_1$ $+$ $(y_1 - y_2 - 3y_3 - y_4)x_2$ $+$ $(2y_1 + y_2 + y_4)x_3$ $\ge$ $6y_1 + 4y_2 + 2y_3 + 7y_4$.
Now, if each of factor of $x_1, x_2, x_3$ is less or greater than original problem then it should be bigger than before.

Therefore, $-x_1 + 3x_2 + x_3$ $\ge$ $(y_2 + y_3 + 2y_4)x_1$ $+$ $(y_1 - y_2 - 3y_3 - y_4)x_2$ $+$ $(2y_1 + y_2 + y_4)x_3$ should hold if followings are true.

1. $y_2 + y_3 + 2y_4$ $\le$ $-1$ because $x_1$ $\ge$ $0$
2. $y_1 - y_2 - 3y_3 - y_4$ $\ge$ $3$ because $x_2$ $\le$ $0$
3. $2y_1 + y_2 + y_4$ $=$ $1$ because there was no limitation for $x_3$ 

Then $-x_1 + 3x_2 + x_3$ $\ge$ $6y_1 + 4y_2 + 2y_3 + 7y_4$ is true.

As a result, we can define a maximization problem of lower bound of minization problem like follow.

Maximize $6y_1 + 4y_2 + 2y_3 + 7y_4$ such that <br>
    $y_2 + y_3 + 2y_4$ $\le$ $-1$,<br>
    $y_1 - y_2 - 3y_3 - y_4$ $\ge$ $3$,<br>
    $2y_1 + y_2 + y_4$ $=$ $1$,<br>
    $y_1 \ge 0$,<br>
    $y_2 \ge 0$,<br>
    $y_3 \le 0$,<br>
    No limitation for $y_4$.

Notice that optimum of this maximization problem is $1$ with $(y_1, y_2, y_3, y_4) = (\frac{5}{9}, 0, -\frac{7}{9}, -\frac{1}{9})$.

Like the process above, we can change the problem to dual of it which finds lower or upper bound of primal.

### Weak duality

Let's assume that primal problem is a minimzation problem for $C^T X$ and dual of it is a maximization problem for $B^T Y$.
Now, let's define optimum of primal as $X$ and optimum of dual as $Y$.
Then, $C^T X \ge B^T Y$ is valid because optimum of dual is a lower bound of primal.

### Strong duality

With the above, there is a stronger statement for it.
If there is a fesible optimum for both primal and dual, $C^T X = B^T Y$.
Proof will be updated later.

Notice that there are three type of situations for LP.
1. LP has an optimum.
2. LP has no feasible solution.
3. LP has unbounded solution.(Infinitely small or big)

For each case, the possible situation is like follow.

|                                | Primal has an optimum | Primal has no feasible solution | Primal has unbounded solution |
| :------           | :------           | :------                 | :------                 |
| Dual has an optimum             |  Possible   |  Impossible |  Impossible |
| Dual has no feasible solution   |  Impossible | Possible | Possible |
| Dual has unbounded solution     |  Impossible | Possible |  Impossible |

### Complementary slackness
For given vectors
$X = \begin{pmatrix} x_1 \\\ x_2 \\\ \vdots \\\ x_n \end{pmatrix}$, 
$Y = \begin{pmatrix} y_1 \\\ y_2 \\\ \vdots \\\ y_m \end{pmatrix}$, 
$C = \begin{pmatrix} c_1 \\\ c_2 \\\ \vdots \\\ c_n \end{pmatrix}$,
$B = \begin{pmatrix} b_1 \\\ b_2 \\\ \vdots \\\ b_m \end{pmatrix}$ and matrix 
$A = \begin{pmatrix} a_{11} & a_{12} & \cdots & a_{1n} \\\ a_{21} & a_{22} & \cdots & a_{2n} \\\ \vdots & \vdots & \ddots & \vdots \\\ a_{m1} & a_{m2} & \cdots & a_{mn} \end{pmatrix}$ $=$ $\begin{pmatrix} A_1 & A_2 & \cdots & A_n \end{pmatrix}$ $=$ $\begin{pmatrix} a_1 \\\ a_2 \\\ \vdots \\\ a_m \end{pmatrix}$.

Primal problem is like follow.

Minimize $C^T X$ such that <br>
    $a_i X \ge b_i$ for $i \in C_p$,<br>
    $a_i X \le b_i$ for $i \in C_m$,<br>
    $a_i X = b_i$ for $i \in C_e$,<br>
    $x_i \ge 0$ for $i \in B_p$,<br>
    $x_i \le 0$ for $i \in B_m$,<br>
    No limitation for $x_i$ for $i \in B_f$.

With the primal problem, dual is like follow.

Maximize $B^T Y$ such that <br>
    $y_i \ge 0$ for $i \in C_p$,<br>
    $y_i \le 0$ for $i \in C_m$,<br>
    No limitation for $y_i$ for $i \in C_e$,<br>
    $A_i^T Y \le c_i$ for $i \in B_p$,<br>
    $A_i^T Y \ge c_i$ for $i \in B_m$,<br>
    $A_i^T Y = c_i$ for $i \in B_f$.
    
Then, there is so-called complementary slackness condition which is follow.
1. $(a_i X - b_i) y_i = 0$ for all $0 \le i \le m$
2. $x_j(A_j^T Y \le c_j) = 0$ for all $0 \le j \le n$

Now, there is a theorem as known as complementary slackness.
Let $X$ and $Y$ be feasible solutions for primal and dual of LP repectively.
Then, $X$ and $Y$ fulfill complementary slackness condition if and only if they are optimum of primal and dual repectively.
Proof will be updated later.

{: .box-note}
**Reference** David P. Williamson and David B. Shmoys, The Design of Approximation Algorithms.