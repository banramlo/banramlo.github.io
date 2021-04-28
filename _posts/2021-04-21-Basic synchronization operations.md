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
To be a lock, lock operation needs to be a thread specific operation.
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
            $\operatorname{for} j \in \{\text{Possible thread index}\}$<br>
            <div class="algorithm">
                $\operatorname{if} flag[j] = \text{true}$
                <div class="algorithm">
                    $acheive \leftarrow \text{false}$
                </div>
            </div>
        </div>
    </div>
    $\operatorname{function} \operatorname{unlock()}$<br>
    <div class="algorithm">
        $flag[i] \leftarrow \text{false}$<br>
    </div>
</div>
From lock above, it can guarantee at most one thread escape the $\operatorname{lock}$.
If not, $flag[i] = true$ and $flag[j] = \text{true}$ should be true but it can't be true.
Proof is like follow.
$(flag[i] \leftarrow \text{true}) \rightarrow (flag[j] = \text{false})$ is the valid sequence because thread $i$ escapes from lock.
Similarly, $(flag[j] \leftarrow \text{true}) \rightarrow (flag[i] = \text{false})$ is the valid sequence because thread $j$ escapes from lock.
There are 6 possible combination of two sequences like below.

1. $(flag[i] \leftarrow \text{true})$ $\rightarrow $ $(flag[j] = \text{false})$ $\rightarrow $ $(flag[j] \leftarrow \text{true})$ $\rightarrow $ $(flag[i] = \text{false})$
2. $(flag[i] \leftarrow \text{true})$ $\rightarrow $ $(flag[j] \leftarrow \text{true})$ $\rightarrow $ $(flag[j] = \text{false})$ $\rightarrow $ $(flag[i] = \text{false})$
3. $(flag[i] \leftarrow \text{true})$ $\rightarrow $ $(flag[j] \leftarrow \text{true})$ $\rightarrow $ $(flag[i] = \text{false})$ $\rightarrow $ $(flag[j] = \text{false})$
4. $(flag[j] \leftarrow \text{true})$ $\rightarrow $ $(flag[i] \leftarrow \text{true})$ $\rightarrow $ $(flag[j] = \text{false})$ $\rightarrow $ $(flag[i] = \text{false})$
5. $(flag[j] \leftarrow \text{true})$ $\rightarrow $ $(flag[i] \leftarrow \text{true})$ $\rightarrow $ $(flag[i] = \text{false})$ $\rightarrow $ $(flag[j] = \text{false})$
6. $(flag[j] \leftarrow \text{true})$ $\rightarrow $ $(flag[i] = \text{false})$ $\rightarrow $ $(flag[i] \leftarrow \text{true})$ $\rightarrow $ $(flag[j] = \text{false})$

However, none of above can't be true.
Therefore, it guarantees to be mutual exclusive.
However, all threads can't escape lock if two thread makes each flags to $\text{true}$ at the same time.

<div class="algorithm">
    $\operatorname{function} \operatorname{lock()}$<br>
    <div class="algorithm">
        $i \leftarrow \text{index of the thread}$<br>
        $flag[i] \leftarrow \text{true}$<br>
        $acheive \leftarrow \text{false}$<br>
        $v \leftarrow i$<br>
        $\operatorname{while} acheive = \text{false}$<br>
        <div class="algorithm">
            $acheive \leftarrow \text{true}$<br>
            $\operatorname{for} j \in \{\text{Possible thread index}\}$<br>
            <div class="algorithm">
                $\operatorname{if} flag[j] = \text{true} and v = i$
                <div class="algorithm">
                    $acheive \leftarrow \text{false}$
                </div>
            </div>
        </div>
    </div>
    $\operatorname{function} \operatorname{unlock()}$<br>
    <div class="algorithm">
        $flag[i] \leftarrow \text{false}$<br>
    </div>
</div>

However, if we add a global variable $v$ to lock, it changes.
Now it checks $v$ didn't changed and all $flag$s are $\text{false}$.
Notice that it is still satisfies mutal exclusive.
There are 20 possible combination of two sequences like below.

1. $(flag[i] \leftarrow \text{true})$ $\rightarrow $ $(v \leftarrow i)$ $\rightarrow $ $(flag[j] = \text{false}\text{ or }v \neq i)$ $\rightarrow $ $(flag[j] \leftarrow \text{true})$ $\rightarrow $ $(v \leftarrow j)$ $\rightarrow $ $(flag[i] = \text{false}\text{ or }v \neq j)$
2. $(flag[i] \leftarrow \text{true})$ $\rightarrow $ $(v \leftarrow i)$ $\rightarrow $ $(flag[j] \leftarrow \text{true})$ $\rightarrow $ $(flag[j] = \text{false}\text{ or }v \neq i)$ $\rightarrow $ $(v \leftarrow j)$ $\rightarrow $ $(flag[i] = \text{false}\text{ or }v \neq j)$
3. $(flag[i] \leftarrow \text{true})$ $\rightarrow $ $(v \leftarrow i)$ $\rightarrow $ $(flag[j] \leftarrow \text{true})$ $\rightarrow $ $(v \leftarrow j)$ $\rightarrow $ $(flag[j] = \text{false}\text{ or }v \neq i)$ $\rightarrow $ $(flag[i] = \text{false}\text{ or }v \neq j)$
4. $(flag[i] \leftarrow \text{true})$ $\rightarrow $ $(v \leftarrow i)$ $\rightarrow $ $(flag[j] \leftarrow \text{true})$ $\rightarrow $ $(v \leftarrow j)$ $\rightarrow $ $(flag[i] = \text{false}\text{ or }v \neq j)$ $\rightarrow $ $(flag[j] = \text{false}\text{ or }v \neq i)$
5. $(flag[i] \leftarrow \text{true})$ $\rightarrow $ $(flag[j] \leftarrow \text{true})$ $\rightarrow $ $(v \leftarrow i)$ $\rightarrow $ $(flag[j] = \text{false}\text{ or }v \neq i)$ $\rightarrow $ $(v \leftarrow j)$ $\rightarrow $ $(flag[i] = \text{false}\text{ or }v \neq j)$
6. $(flag[i] \leftarrow \text{true})$ $\rightarrow $ $(flag[j] \leftarrow \text{true})$ $\rightarrow $ $(v \leftarrow i)$ $\rightarrow $ $(v \leftarrow j)$ $\rightarrow $ $(flag[j] = \text{false}\text{ or }v \neq i)$ $\rightarrow $ $(flag[i] = \text{false}\text{ or }v \neq j)$
7. $(flag[i] \leftarrow \text{true})$ $\rightarrow $ $(flag[j] \leftarrow \text{true})$ $\rightarrow $ $(v \leftarrow i)$ $\rightarrow $ $(v \leftarrow j)$ $\rightarrow $ $(flag[i] = \text{false}\text{ or }v \neq j)$ $\rightarrow $ $(flag[j] = \text{false}\text{ or }v \neq i)$
8. $(flag[i] \leftarrow \text{true})$ $\rightarrow $ $(flag[j] \leftarrow \text{true})$ $\rightarrow $ $(v \leftarrow j)$ $\rightarrow $ $(v \leftarrow i)$ $\rightarrow $ $(flag[j] = \text{false}\text{ or }v \neq i)$ $\rightarrow $ $(flag[i] = \text{false}\text{ or }v \neq j)$
9. $(flag[i] \leftarrow \text{true})$ $\rightarrow $ $(flag[j] \leftarrow \text{true})$ $\rightarrow $ $(v \leftarrow j)$ $\rightarrow $ $(v \leftarrow i)$ $\rightarrow $ $(flag[i] = \text{false}\text{ or }v \neq j)$ $\rightarrow $ $(flag[j] = \text{false}\text{ or }v \neq i)$
10. $(flag[i] \leftarrow \text{true})$ $\rightarrow $ $(flag[j] \leftarrow \text{true})$ $\rightarrow $ $(v \leftarrow j)$ $\rightarrow $ $(flag[i] = \text{false}\text{ or }v \neq j)$ $\rightarrow $ $(v \leftarrow i)$ $\rightarrow $ $(flag[j] = \text{false}\text{ or }v \neq i)$
11. $(flag[j] \leftarrow \text{true})$ $\rightarrow $ $(flag[i] \leftarrow \text{true})$ $\rightarrow $ $(v \leftarrow i)$ $\rightarrow $ $(flag[j] = \text{false}\text{ or }v \neq i)$ $\rightarrow $ $(v \leftarrow j)$ $\rightarrow $ $(flag[i] = \text{false}\text{ or }v \neq j)$
12. $(flag[j] \leftarrow \text{true})$ $\rightarrow $ $(flag[i] \leftarrow \text{true})$ $\rightarrow $ $(v \leftarrow i)$ $\rightarrow $ $(v \leftarrow j)$ $\rightarrow $ $(flag[j] = \text{false}\text{ or }v \neq i)$ $\rightarrow $ $(flag[i] = \text{false}\text{ or }v \neq j)$
13. $(flag[j] \leftarrow \text{true})$ $\rightarrow $ $(flag[i] \leftarrow \text{true})$ $\rightarrow $ $(v \leftarrow i)$ $\rightarrow $ $(v \leftarrow j)$ $\rightarrow $ $(flag[i] = \text{false}\text{ or }v \neq j)$ $\rightarrow $ $(flag[j] = \text{false}\text{ or }v \neq i)$
14. $(flag[j] \leftarrow \text{true})$ $\rightarrow $ $(flag[i] \leftarrow \text{true})$ $\rightarrow $ $(v \leftarrow j)$ $\rightarrow $ $(v \leftarrow i)$ $\rightarrow $ $(flag[j] = \text{false}\text{ or }v \neq i)$ $\rightarrow $ $(flag[i] = \text{false}\text{ or }v \neq j)$
15. $(flag[j] \leftarrow \text{true})$ $\rightarrow $ $(flag[i] \leftarrow \text{true})$ $\rightarrow $ $(v \leftarrow j)$ $\rightarrow $ $(v \leftarrow i)$ $\rightarrow $ $(flag[i] = \text{false}\text{ or }v \neq j)$ $\rightarrow $ $(flag[j] = \text{false}\text{ or }v \neq i)$
16. $(flag[j] \leftarrow \text{true})$ $\rightarrow $ $(flag[i] \leftarrow \text{true})$ $\rightarrow $ $(v \leftarrow j)$ $\rightarrow $ $(flag[i] = \text{false}\text{ or }v \neq j)$ $\rightarrow $ $(v \leftarrow i)$ $\rightarrow $ $(flag[j] = \text{false}\text{ or }v \neq i)$
17. $(flag[j] \leftarrow \text{true})$ $\rightarrow $ $(v \leftarrow j)$ $\rightarrow $ $(flag[i] \leftarrow \text{true})$ $\rightarrow $ $(v \leftarrow i)$ $\rightarrow $ $(flag[j] = \text{false}\text{ or }v \neq i)$ $\rightarrow $ $(flag[i] = \text{false}\text{ or }v \neq j)$
18. $(flag[j] \leftarrow \text{true})$ $\rightarrow $ $(v \leftarrow j)$ $\rightarrow $ $(flag[i] \leftarrow \text{true})$ $\rightarrow $ $(v \leftarrow i)$ $\rightarrow $ $(flag[i] = \text{false}\text{ or }v \neq j)$ $\rightarrow $ $(flag[j] = \text{false}\text{ or }v \neq i)$
19. $(flag[j] \leftarrow \text{true})$ $\rightarrow $ $(v \leftarrow j)$ $\rightarrow $ $(flag[i] \leftarrow \text{true})$ $\rightarrow $ $(flag[i] = \text{false}\text{ or }v \neq j)$ $\rightarrow $ $(v \leftarrow i)$ $\rightarrow $ $(flag[j] = \text{false}\text{ or }v \neq i)$
20. $(flag[j] \leftarrow \text{true})$ $\rightarrow $ $(v \leftarrow j)$ $\rightarrow $ $(flag[i] = \text{false}\text{ or }v \neq j)$ $\rightarrow $ $(flag[i] \leftarrow \text{true})$ $\rightarrow $ $(v \leftarrow i)$ $\rightarrow $ $(flag[j] = \text{false}\text{ or }v \neq i)$

However, none of above can't be true.

## TAS(Test and set)

## TTAS(Test Test and set)

## Atomic

## CAS(Compare and swap)
