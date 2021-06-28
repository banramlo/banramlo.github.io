---
layout: post
title: Merge sort
gh-repo: daattali/beautiful-jekyll
tags: [algorithm]
comments: true
use_math: true
krenable: true
---

<div class="eng">
Merge sort is a naive way to sort with divide and conquer methodology.
It needs $O(n)$ memory space unlike quick sort.
However, it can has only $O(n \log n)$ time complexity even at the worst-case.
</div>
<div class="kor">
Merge sort는 divide and conquer 방법론을 이용한 직관적인 정렬방법입니다.
이 알고리즘은 quick sort와는 다르게 $O(n)$의 메모리 공간을 요구합니다.
하지만, 이 알고리즘은 최악의 경우라도 $O(n \log n)$의 시간 복잡도를 가집니다.
</div>

```python
def _mergeSort(lst, fromIdx, toIdx):
    if toIdx - fromIdx <= 1:
        return
    mid = int((toIdx + fromIdx)/2)
    _mergeSort(lst, fromIdx, mid)
    _mergeSort(lst, mid, toIdx)

    #Where extra space is needed.
    auxList = [0] * (toIdx - fromIdx)
    idx1 = fromIdx
    idx2 = mid
    for i in range(toIdx - fromIdx):
        if idx2 >= toIdx or idx1 < mid and lst[idx1] <= lst[idx2]:#choose idx1
            auxList[i] = lst[idx1]
            idx1 += 1
        elif idx1 >= mid or idx2 < toIdx and lst[idx2] < lst[idx1]:#choose idx2
            auxList[i] = lst[idx2]
            idx2 += 1
        else:
            print('Invalid situation')
            raise
    for i in range(toIdx - fromIdx):
        lst[i + fromIdx] = auxList[i]
        
def mergeSort(lst):
    return _mergeSort(lst, 0, len(lst))

#Test code
import random

if __name__ == "__main__":
    for i in range(1000):
        lst = []
        for j in range(1000):
            lst.append(random.randint(0,100))
        targetIdx = random.randint(0,len(lst) - 1)
        lst2 = list(lst)

        #Will crush the qsort
        #lst2[0] = 0

        mergeSort(lst2)
        if sorted(lst) != lst2:
            print('wrong result for qsort')
            raise
    print('Every thing done well!')
```

{: .box-note}
**Try this now:** <a href="http://pythontutor.com/visualize.html#code=def%20_mergeSort%28lst,%20fromIdx,%20toIdx%29%3A%0A%20%20%20%20if%20toIdx%20-%20fromIdx%20%3C%3D%201%3A%0A%20%20%20%20%20%20%20%20return%0A%20%20%20%20mid%20%3D%20int%28%28toIdx%20%2B%20fromIdx%29/2%29%0A%20%20%20%20_mergeSort%28lst,%20fromIdx,%20mid%29%0A%20%20%20%20_mergeSort%28lst,%20mid,%20toIdx%29%0A%0A%20%20%20%20%23Where%20extra%20space%20is%20needed.%0A%20%20%20%20auxList%20%3D%20%5B0%5D%20*%20%28toIdx%20-%20fromIdx%29%0A%20%20%20%20idx1%20%3D%20fromIdx%0A%20%20%20%20idx2%20%3D%20mid%0A%20%20%20%20for%20i%20in%20range%28toIdx%20-%20fromIdx%29%3A%0A%20%20%20%20%20%20%20%20if%20idx2%20%3E%3D%20toIdx%20or%20idx1%20%3C%20mid%20and%20lst%5Bidx1%5D%20%3C%3D%20lst%5Bidx2%5D%3A%23choose%20idx1%0A%20%20%20%20%20%20%20%20%20%20%20%20auxList%5Bi%5D%20%3D%20lst%5Bidx1%5D%0A%20%20%20%20%20%20%20%20%20%20%20%20idx1%20%2B%3D%201%0A%20%20%20%20%20%20%20%20elif%20idx1%20%3E%3D%20mid%20or%20idx2%20%3C%20toIdx%20and%20lst%5Bidx2%5D%20%3C%20lst%5Bidx1%5D%3A%23choose%20idx2%0A%20%20%20%20%20%20%20%20%20%20%20%20auxList%5Bi%5D%20%3D%20lst%5Bidx2%5D%0A%20%20%20%20%20%20%20%20%20%20%20%20idx2%20%2B%3D%201%0A%20%20%20%20%20%20%20%20else%3A%0A%20%20%20%20%20%20%20%20%20%20%20%20print%28'Invalid%20situation'%29%0A%20%20%20%20%20%20%20%20%20%20%20%20raise%0A%20%20%20%20for%20i%20in%20range%28toIdx%20-%20fromIdx%29%3A%0A%20%20%20%20%20%20%20%20lst%5Bi%20%2B%20fromIdx%5D%20%3D%20auxList%5Bi%5D%0A%20%20%20%20%20%20%20%20%0Adef%20mergeSort%28lst%29%3A%0A%20%20%20%20return%20_mergeSort%28lst,%200,%20len%28lst%29%29%0A%0A%23Test%20code%0Aimport%20random%0A%0Alst%20%3D%20%5B%5D%0Afor%20j%20in%20range%2820%29%3A%0A%20%20%20%20lst.append%28random.randint%280,100%29%29%0AtargetIdx%20%3D%20random.randint%280,len%28lst%29%20-%201%29%0Alst2%20%3D%20list%28lst%29%0A%0A%23Will%20crush%20the%20qsort%0A%23lst2%5B0%5D%20%3D%200%0A%0AmergeSort%28lst2%29%0Aif%20sorted%28lst%29%20!%3D%20lst2%3A%0A%20%20%20%20print%28'wrong%20result%20for%20qsort'%29%0A%20%20%20%20raise%0Aprint%28'Every%20thing%20done%20well!'%29&cumulative=false&heapPrimitives=nevernest&mode=edit&origin=opt-frontend.js&py=3&rawInputLstJSON=%5B%5D&textReferences=false">Python tutor</a> 
