---
layout: post
title: Family of algorithms
gh-repo: daattali/beautiful-jekyll
tags: [algorithm, approximation]
comments: true
use_math: true
---

There are some algorithm families.

## PTAS(Polynomial-time approximation scheme)
$\operatorname{PTAS}$ is a family of algorithms which is a $(1 + \epsilon)$ - approximation for minimization problem and a $(1 - \epsilon)$ - approximation for maximization problem.
Notice that running time may depend on $\epsilon$.
However, without polynomial restriction for it.

## FPAS, FPTAS(Fully polynomial-time approximation scheme)
$\operatorname{FPAS}$ is a family of algorithms which is $\operatorname{PTAS}$ and running time is also bounded to a polynomial in $\frac{1}{\epsilon}$.

{: .box-note}
**Reference** David P. Williamson and David B. Shmoys, The Design of Approximation Algorithms.