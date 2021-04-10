---
layout: post
title: Quick sort and quick select
gh-repo: daattali/beautiful-jekyll
tags: [algorithm]
comments: true
use_math: true
---

Quick sort and quick selection is based on the partition algorithm.
Partition algorithm makes a two sublist($\mathcal{A},\mathcal{B}$) and one value(pivot$ = \gamma$).
Every element in $\mathcal{A}$ is less or equal than $\gamma$.
Every element in $\mathcal{B}$ is greater than $\gamma$.
Notice that $\mathcal{A}$ can have some elements which have same value with $\gamma$ but not $\gamma$ itself and $\mathcal{A},\mathcal{B}$ may be $\emptyset$.
As a result, partition function makes $\gamma$ is the highest value of $\mathcal{A}$.
qsort uses this property to sort the list.
After partition function, $\mathcal{A}$ and $\mathcal{B}$ is sorted to each other by $\gamma$.
Therefore, it just sort recursively in $\mathcal{A}$ and $\mathcal{B}$ by qsort.
qselect do the simillar thing, it checks $\gamma$'s order.
If the order of $\gamma$ is less or equal than what seek for, do partition at $\mathcal{B}$ and try to check $\gamma$ again.
If the order of $\gamma$ is greater than what seek for, do partition at $\mathcal{A}$ and try to check $\gamma$ again.
It works like semi-qsort.
It sorts a little bit and tries to find the elements in $k$th order.

```python
def partition(lst, fromIdx, toIdx):
    '''
    Q-sort partition algorithm
    [fromIdx,   toIdx)
    inclusive, exclusive

    It is inplace, mutatable function
    return list partition point of pivot
    (smaller or equal than pivot) (pivot) (larger than pivot)
    '''
    selectPoint = int((toIdx + fromIdx)/2)
    lst[fromIdx],lst[selectPoint] = lst[selectPoint],lst[fromIdx]
    criteria = lst[fromIdx]
    lIdx = fromIdx
    rIdx = toIdx - 1
    while lIdx < rIdx:
        while lIdx < rIdx:
            if lst[lIdx] > criteria:
                break
            lIdx += 1
        if lIdx == rIdx:
            break
        while lIdx < rIdx:
            if lst[rIdx] <= criteria:
                break
            rIdx -= 1
        lst[lIdx], lst[rIdx] = lst[rIdx], lst[lIdx]
    if lst[lIdx] > lst[fromIdx]:
        lIdx -= 1
    lst[lIdx], lst[fromIdx] = lst[fromIdx], lst[lIdx]
    return lIdx

def _qsort(lst, fromIdx, toIdx):
    if toIdx - fromIdx <= 1:
        return
    mid = partition(lst, fromIdx, toIdx)
    _qsort(lst, fromIdx, mid)
    _qsort(lst, mid + 1, toIdx)
    
def qsort(lst):
    _qsort(lst, 0, len(lst))

def _qselect(lst, idx, fromIdx, toIdx):
    if toIdx - fromIdx <= 1:
        return
    mid = partition(lst, fromIdx, toIdx)
    if mid == idx:
        return
    if idx < mid: # target is inside of left handSide
        _qselect(lst, idx, fromIdx, mid)
    else:
        _qselect(lst, idx, mid + 1, toIdx)
    
    
def qselect(lst, idx):
    if len(lst) <= idx:
        return -1
    _qselect(lst, idx, 0, len(lst))
    return lst[idx]

#Test code
import random

if __name__ == "__main__":
    for i in range(1000):
        lst = []
        for j in range(1000):
            lst.append(random.randint(0,100))
        targetIdx = random.randint(0,len(lst) - 1)
        lst2 = list(lst)

        #Highly crush the qsort
        #lst2[0] = 0

        qsort(lst2)
        if sorted(lst) != lst2:
            print('wrong result for qsort')
            raise
        
        #Lowly crush the qselect
        #lst[0] = lst[0] - 1
        #Highly crush the qselect
        #lst[0] = lst[0] - 20
        if qselect(lst, targetIdx) != lst2[targetIdx]:
            print('wrong result for qselect')
            raise
    print('Every thing done well!')

```

{: .box-note}
**Try this now:** <a href="http://pythontutor.com/visualize.html#code=def%20partition%28lst,%20fromIdx,%20toIdx%29%3A%0A%20%20%20%20'''%0A%20%20%20%20Q-sort%20partition%20algorithm%0A%20%20%20%20%5BfromIdx,%20%20%20toIdx%29%0A%20%20%20%20inclusive,%20exclusive%0A%0A%20%20%20%20It%20is%20inplace,%20mutatable%20function%0A%20%20%20%20return%20list%20partition%20point%20of%20pivot%0A%20%20%20%20%28smaller%20or%20equal%20than%20pivot%29%20%28pivot%29%20%28larger%20than%20pivot%29%0A%20%20%20%20'''%0A%20%20%20%20selectPoint%20%3D%20int%28%28toIdx%20%2B%20fromIdx%29/2%29%0A%20%20%20%20lst%5BfromIdx%5D,lst%5BselectPoint%5D%20%3D%20lst%5BselectPoint%5D,lst%5BfromIdx%5D%0A%20%20%20%20criteria%20%3D%20lst%5BfromIdx%5D%0A%20%20%20%20lIdx%20%3D%20fromIdx%0A%20%20%20%20rIdx%20%3D%20toIdx%20-%201%0A%20%20%20%20while%20lIdx%20%3C%20rIdx%3A%0A%20%20%20%20%20%20%20%20while%20lIdx%20%3C%20rIdx%3A%0A%20%20%20%20%20%20%20%20%20%20%20%20if%20lst%5BlIdx%5D%20%3E%20criteria%3A%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20break%0A%20%20%20%20%20%20%20%20%20%20%20%20lIdx%20%2B%3D%201%0A%20%20%20%20%20%20%20%20if%20lIdx%20%3D%3D%20rIdx%3A%0A%20%20%20%20%20%20%20%20%20%20%20%20break%0A%20%20%20%20%20%20%20%20while%20lIdx%20%3C%20rIdx%3A%0A%20%20%20%20%20%20%20%20%20%20%20%20if%20lst%5BrIdx%5D%20%3C%3D%20criteria%3A%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20break%0A%20%20%20%20%20%20%20%20%20%20%20%20rIdx%20-%3D%201%0A%20%20%20%20%20%20%20%20lst%5BlIdx%5D,%20lst%5BrIdx%5D%20%3D%20lst%5BrIdx%5D,%20lst%5BlIdx%5D%0A%20%20%20%20if%20lst%5BlIdx%5D%20%3E%20lst%5BfromIdx%5D%3A%0A%20%20%20%20%20%20%20%20lIdx%20-%3D%201%0A%20%20%20%20lst%5BlIdx%5D,%20lst%5BfromIdx%5D%20%3D%20lst%5BfromIdx%5D,%20lst%5BlIdx%5D%0A%20%20%20%20return%20lIdx%0A%0Adef%20_qsort%28lst,%20fromIdx,%20toIdx%29%3A%0A%20%20%20%20if%20toIdx%20-%20fromIdx%20%3C%3D%201%3A%0A%20%20%20%20%20%20%20%20return%0A%20%20%20%20mid%20%3D%20partition%28lst,%20fromIdx,%20toIdx%29%0A%20%20%20%20_qsort%28lst,%20fromIdx,%20mid%29%0A%20%20%20%20_qsort%28lst,%20mid%20%2B%201,%20toIdx%29%0A%20%20%20%20%0Adef%20qsort%28lst%29%3A%0A%20%20%20%20_qsort%28lst,%200,%20len%28lst%29%29%0A%0Adef%20_qselect%28lst,%20idx,%20fromIdx,%20toIdx%29%3A%0A%20%20%20%20if%20toIdx%20-%20fromIdx%20%3C%3D%201%3A%0A%20%20%20%20%20%20%20%20return%0A%20%20%20%20mid%20%3D%20partition%28lst,%20fromIdx,%20toIdx%29%0A%20%20%20%20if%20mid%20%3D%3D%20idx%3A%0A%20%20%20%20%20%20%20%20return%0A%20%20%20%20if%20idx%20%3C%20mid%3A%20%23%20target%20is%20inside%20of%20left%20handSide%0A%20%20%20%20%20%20%20%20_qselect%28lst,%20idx,%20fromIdx,%20mid%29%0A%20%20%20%20else%3A%0A%20%20%20%20%20%20%20%20_qselect%28lst,%20idx,%20mid%20%2B%201,%20toIdx%29%0A%20%20%20%20%0A%20%20%20%20%0Adef%20qselect%28lst,%20idx%29%3A%0A%20%20%20%20if%20len%28lst%29%20%3C%3D%20idx%3A%0A%20%20%20%20%20%20%20%20return%20-1%0A%20%20%20%20_qselect%28lst,%20idx,%200,%20len%28lst%29%29%0A%20%20%20%20return%20lst%5Bidx%5D%0A%20%20%20%20%0A%23Test%20code%0Aimport%20random%0A%0Alst%20%3D%20%5B%5D%0Afor%20j%20in%20range%2820%29%3A%0A%20%20%20%20lst.append%28random.randint%280,100%29%29%0AtargetIdx%20%3D%20random.randint%280,len%28lst%29%20-%201%29%0Alst2%20%3D%20list%28lst%29%0A%0Awhile%20True%3A%0A%20%20%20%20option%20%3D%20input%28'sort%28s%29%20or%20select%28e%29%3A'%29%0A%20%20%20%20if%20option%20%3D%3D%20's'%3A%0A%20%20%20%20%20%20%20%20%23Highly%20crush%20the%20qsort%0A%20%20%20%20%20%20%20%20%23lst2%5B0%5D%20%3D%200%0A%20%20%20%20%20%20%20%20%0A%20%20%20%20%20%20%20%20qsort%28lst2%29%0A%20%20%20%20%20%20%20%20if%20sorted%28lst%29%20!%3D%20lst2%3A%0A%20%20%20%20%20%20%20%20%20%20%20%20print%28'wrong%20result%20for%20qsort'%29%0A%20%20%20%20%20%20%20%20%20%20%20%20raise%0A%20%20%20%20%20%20%20%20print%28'Every%20thing%20done%20well!'%29%0A%20%20%20%20%20%20%20%20break%0A%20%20%20%20elif%20option%20%3D%3D%20'e'%3A%0A%20%20%20%20%20%20%20%20%23Lowly%20crush%20the%20qselect%0A%20%20%20%20%20%20%20%20%23lst%5B0%5D%20%3D%20lst%5B0%5D%20-%201%0A%20%20%20%20%20%20%20%20%23Highly%20crush%20the%20qselect%0A%20%20%20%20%20%20%20%20%23lst%5B0%5D%20%3D%20lst%5B0%5D%20-%2020%0A%20%20%20%20%20%20%20%20lst2%20%3D%20sorted%28lst%29%0A%20%20%20%20%20%20%20%20if%20qselect%28lst,%20targetIdx%29%20!%3D%20lst2%5BtargetIdx%5D%3A%0A%20%20%20%20%20%20%20%20%20%20%20%20print%28'wrong%20result%20for%20qselect'%29%0A%20%20%20%20%20%20%20%20%20%20%20%20raise%0A%20%20%20%20%20%20%20%20print%28'Every%20thing%20done%20well!'%29%0A%20%20%20%20%20%20%20%20break&cumulative=false&heapPrimitives=nevernest&mode=edit&origin=opt-frontend.js&py=3&rawInputLstJSON=%5B%22e%22%5D&textReferences=false">Python tutor</a> 
