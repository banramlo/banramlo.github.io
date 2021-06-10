---
layout: post
title: Kronecker product
gh-repo: daattali/beautiful-jekyll
tags: [algorithm, graph]
comments: true
use_math: true
---

Kronecker product is a mathmatical tools to find all possible combination of something.
It can be used for many combinational problem like Traveling-salesman problem.

Now, kronecker product $\otimes$ defines like follow.

For $A$ $=$ $\begin{pmatrix}A_{1,1} & A_{1,2} & \cdots & A_{1,n}\\\\ A_{2,1} & A_{2,2} & \cdots & A_{2,n}\\\\ \vdots & \vdots & \ddots & \vdots\\\\ A_{m,1} & A_{m,2} & \cdots & A_{m,n}\\\\ \end{pmatrix}$,
$B$ $=$ $\begin{pmatrix}B_{1,1} & B_{1,2} & \cdots & B_{1,p}\\\\ B_{2,1} & B_{2,2} & \cdots & B_{2,p}\\\\ \vdots & \vdots & \ddots & \vdots\\\\ B_{q,1} & B_{q,2} & \cdots & B_{q,p}\\\\ \end{pmatrix}$,

$A \otimes B$ $=$ $\begin{pmatrix}A_{1,1}B & A_{1,2}B & \cdots & A_{1,n}B\\\\ A_{2,1}B & A_{2,2}B & \cdots & A_{2,n}B\\\\ \vdots & \vdots & \ddots & \vdots\\\\ A_{m,1}B & A_{m,2}B & \cdots & A_{m,n}B\\\\ \end{pmatrix}$ $=$ 
$\begin{pmatrix}A_{1,1}B_{1,1} & A_{1,1}B_{1,2} & \cdots & A_{1,1}B_{1,p} & A_{1,2}B_{1,1} & A_{1,2}B_{1,2} & \cdots & A_{1,2}B_{1,p} & A_{1,3}B_{1,1} & \cdots & A_{1,n}B_{1,p}\\\\ A_{1,1}B_{2,1} & A_{1,1}B_{2,2} & \cdots & A_{1,1}B_{2,p} & A_{1,2}B_{2,1} & A_{1,2}B_{2,2} & \cdots & A_{1,2}B_{2,p} & A_{1,3}B_{2,1} & \cdots & A_{1,n}B_{2,p}\\\\ \vdots         & \vdots         & \ddots & \vdots         & \vdots         & \vdots         & \ddots & \vdots         & \vdots         & \ddots & \vdots        \\\\ A_{1,1}B_{q,1} & A_{1,1}B_{q,2} & \cdots & A_{1,1}B_{q,p} & A_{1,2}B_{q,1} & A_{1,2}B_{q,2} & \cdots & A_{1,2}B_{q,p} & A_{1,3}B_{q,1} & \cdots & A_{1,n}B_{q,p}\\\\ A_{2,1}B_{1,1} & A_{2,1}B_{1,2} & \cdots & A_{2,1}B_{1,p} & A_{2,2}B_{1,1} & A_{2,2}B_{1,2} & \cdots & A_{2,2}B_{1,p} & A_{2,3}B_{1,1} & \cdots & A_{2,n}B_{1,p}\\\\ A_{2,1}B_{2,1} & A_{2,1}B_{2,2} & \cdots & A_{2,1}B_{2,p} & A_{2,2}B_{2,1} & A_{2,2}B_{2,2} & \cdots & A_{2,2}B_{2,p} & A_{2,3}B_{2,1} & \cdots & A_{2,n}B_{2,p}\\\\ \vdots         & \vdots         & \ddots & \vdots         & \vdots         & \vdots         & \ddots & \vdots         & \vdots         & \ddots & \vdots        \\\\ A_{2,1}B_{q,1} & A_{2,1}B_{q,2} & \cdots & A_{2,1}B_{q,p} & A_{2,2}B_{q,1} & A_{2,2}B_{q,2} & \cdots & A_{2,2}B_{q,p} & A_{2,3}B_{q,1} & \cdots & A_{2,n}B_{q,p}\\\\ \vdots         & \vdots         & \ddots & \vdots         & \vdots         & \vdots         & \ddots & \vdots         & \vdots         & \ddots & \vdots        \\\\ A_{m,1}B_{q,1} & A_{m,1}B_{q,2} & \cdots & A_{m,1}B_{q,p} & A_{m,2}B_{q,1} & A_{m,2}B_{q,2} & \cdots & A_{m,2}B_{q,p} & A_{m,3}B_{q,1} & \cdots & A_{m,n}B_{q,p}\\\\ \end{pmatrix}$.

Notice that this can be used for something recursive because it copies $B$s to submatrices.