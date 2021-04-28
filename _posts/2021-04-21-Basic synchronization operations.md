---
layout: post
title: Basic synchronization operations
gh-repo: daattali/beautiful-jekyll
tags: [algorithm, approximation]
comments: true
use_math: true
---

Synchronizing threads are not trivial to achieve.
There are some techniques to achieve this property.

1. Lock
2. TAS(Test and set)
3. TTAS(Test Test and set)
4. Atomic
5. CAS(Compare and swap)

## Lock
Lock is a variable that can be locked and unlocked.
To be a lock, lock need to be a thread specific operation to be achieved.
There are some methods to make locks.
Notice that every implementation below need to be un-reordered.

<div class="algorithm">
    $\operatorname{function} \operatorname{lock()}$<br>
    <div class="algorithm">
        $i \leftarrow \text{index of the thread}$<br>
        $flag[i] \leftarrow \text{true}$<br>
        $acheive \leftarrow \text{false}$<br>
        $\operatorname{while} acheive = \text{false}$<br>
        <div class="algorithm">
            $acheive \leftarrow \text{true}$<br>
            $\operatorname{for} j \in \\{Possible thread index\\}$<br>
            <div class="algorithm">
                $\operatorname{if} flag[j] = \text{true}$
                <div class="algorithm">
                    $acheive \leftarrow \text{false}$
                </div>
            </div>
        </div>
    </div>
</div>

## TAS(Test and set)

## TTAS(Test Test and set)

## Atomic

## CAS(Compare and swap)
