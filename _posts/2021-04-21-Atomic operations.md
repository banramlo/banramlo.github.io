---
layout: post
title: Atomic operations
gh-repo: daattali/beautiful-jekyll
tags: [algorithm, approximation]
comments: true
use_math: true
---

Atomic operation has been an import operation for a parallel computation in CPU or GPU.
Atomic operations guarantess to be executed in a single step.
There are some of atomic operations which are typicaly supported by CPU.
This post will be updated if something hot comes new.

## TAS(Test and set)

TAS is an atomic operation that reads one bit at the specific location and write 1 if that was 0 before.
It will return $\text{true}$ if it writes and $\text{false}$ if it failed to write.

## CAS(Compare and swap)

CAS is an atomic operation that read some variable, check whether it is the same with asking and write if it is the same.
If it is the same with what asking, it writes a new value and return $\text{true}$.
If the value is not the same with what asking, update variable to ask with newer value and return $\text{false}$.