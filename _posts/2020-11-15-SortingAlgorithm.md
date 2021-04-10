---
layout: post
title: Sorting algorithm analysis
gh-repo: daattali/beautiful-jekyll
tags: [algorithm]
comments: true
use_math: true
---

From the previous algorithms, qsort is the slowest at the worst cases and others are always $O(n logn)$.
However, qsort is known to be fastest at the most of cases.
From the experiment below, it is shown as known.
In a summury, it forms like below.

| Algorithm | Average speed | Worst speed | Extra memory usage |
| :------ |:--- | :--- | :--- |
| Qsort | Fastest($O(n log n)$) | Slowest($O(n^2)$) | None($O(c)$) |
| Merge sort | Normal($O(n log n)$) | Normal($O(n log n)$) |  Auxlist($O(n)$) |
| Heap sort | Normal($O(n log n)$) | Normal($O(n log n)$) | None($O(c)$) |

<div align="center" id="sortResultPlot1">
</div>

<script>
    function readData(url, result){
        var thisText = new XMLHttpRequest(url);
        thisText.open("GET", url, false);
        thisText.setRequestHeader("Content-Type", "text");
        thisText.send(null);
        var body = thisText.responseText.split('\n');
        for(let i = 0; i < body.length; ++i){
            let line = body[i].trim();
            if(line != ''){
                let points = line.split(' ');
                result['x'].push(parseFloat(points[0]));
                result['y'].push(parseFloat(points[1]));
            }
        }
    }
    var heapRes = {name: 'Heap sort', type: 'scatter', x: [],y: []};
    var mergeRes = {name: 'Merge sort', type: 'scatter', x: [],y: []};
    var qRes = {name: 'Quick sort', type: 'scatter', x: [],y: []};
    readData("https://banramlo.github.io/assets/post/2020-11-15-SortingAlgorithm/heapSort.dat", heapRes);
    readData("https://banramlo.github.io/assets/post/2020-11-15-SortingAlgorithm/mergeSort.dat", mergeRes);
    readData("https://banramlo.github.io/assets/post/2020-11-15-SortingAlgorithm/qSort.dat", qRes);
    var data = [heapRes, mergeRes, qRes];
    var layout = {
        xaxis: {
            title: '# elements',
            showgrid: false,
            zeroline: false
        },
        yaxis: {
            title: 'Execution time(sec)',
            type: 'log',
            showline: false
        }
    };
    Plotly.newPlot('sortResultPlot1', data, layout);
</script>

```c
#include <iostream>
#include <math.h>
#include <time.h>
#include <vector>
#include <algorithm>
#include <fstream>
#include <chrono>

void swap(int& a, int& b){
    int c = a;
    a = b;
    b = c;
}

void downHeap(int lst[], int length, int idx){
    if (2 * idx + 2 < length){
        if (lst[idx] < lst[2 * idx + 1]){
            if(lst[2 * idx + 1] < lst[2 * idx + 2]){
                swap(lst[idx], lst[2 * idx + 2]);
                downHeap(lst, length, 2 * idx + 2);
            }
            else{
                swap(lst[idx], lst[2 * idx + 1]);
                downHeap(lst, length, 2 * idx + 1);
            }
        }
        else if (lst[idx] < lst[2 * idx + 2]){
            swap(lst[idx], lst[2 * idx + 2]);
            downHeap(lst,length, 2 * idx + 2);
        }
    }
    else if (2 * idx + 1 < length){
        if (lst[idx] < lst[2 * idx + 1]){
            swap(lst[idx], lst[2 * idx + 1]);
            downHeap(lst, length, 2 * idx + 1);
        }
    }
}

void listToHeap(int lst[], int length){
    int height = 0;
    int sumE   = 0;
    while (sumE < length){
        height += 1;
        sumE   *= 2;
        sumE   += 1;
    }
    for(int i = height - 2; i > -1; i -= 1){
        for(int j = pow(2,i) -1; j < pow(2,i+1) -1; j++){
            downHeap(lst, length, j);
        }
    }
}

void heapSort(int lst[], int length){
    listToHeap(lst, length);
    for(int i = length-1; i > 0; --i){
        swap(lst[0], lst[i]);
        downHeap(lst, i, 0);
    }
}

int partition(int lst[], int fromIdx, int toIdx){
    int selectPoint = (toIdx + fromIdx)/2;
    swap(lst[fromIdx], lst[selectPoint]);
    int criteria = lst[fromIdx];
    int lIdx = fromIdx;
    int rIdx = toIdx - 1;
    while(lIdx < rIdx){
        while(lIdx < rIdx){
            if(lst[lIdx] > criteria)
                break;
            lIdx += 1;
        }
        if(lIdx == rIdx)
            break;
        while (lIdx < rIdx){
            if (lst[rIdx] <= criteria)
                break;
            rIdx -= 1;
        }
        swap(lst[lIdx], lst[rIdx]);
    }
    if (lst[lIdx] > lst[fromIdx])
        lIdx -= 1;
    swap(lst[lIdx], lst[fromIdx]);
    return lIdx;
}

void _qsort(int lst[], int fromIdx, int toIdx){
    if(toIdx - fromIdx <= 1)
        return;
    int mid = partition(lst, fromIdx, toIdx);
    _qsort(lst, fromIdx, mid);
    _qsort(lst, mid + 1, toIdx);
}
    
void qsort(int lst[], int length){
    _qsort(lst, 0, length);
}

void _mergeSort(int lst[], int fromIdx, int toIdx){
    if (toIdx - fromIdx <= 1)
        return;
    int mid = (toIdx + fromIdx)/2;
    _mergeSort(lst, fromIdx, mid);
    _mergeSort(lst, mid, toIdx);

    int auxList[toIdx - fromIdx];
    int idx1 = fromIdx;
    int idx2 = mid;
    for(int i = 0; i < (toIdx - fromIdx); ++i){
        if (idx2 >= toIdx || idx1 < mid && lst[idx1] <= lst[idx2]){
            auxList[i] = lst[idx1];
            idx1 += 1;
        }
        else if (idx1 >= mid || idx2 < toIdx && lst[idx2] < lst[idx1]){
            auxList[i] = lst[idx2];
            idx2 += 1;
        }
        else{
            std::cout << "Invalid situation" << std::endl;
            exit(1);
        }
    }
    for(int i = 0; i < (toIdx - fromIdx); ++i){
        lst[i + fromIdx] = auxList[i];
    }
}
        
void mergeSort(int lst[], int length){
    return _mergeSort(lst, 0, length);
}


int main(){
    using namespace std;

    srand(time(NULL));
    std::chrono::duration<double> mergeTime;
    std::chrono::duration<double> qTime;
    std::chrono::duration<double> heapTime;
    
    ofstream qDat("qSort.dat"), mDat("mergeSort.dat"), hDat("heapSort.dat");
    for(int i = 0; i < 1000; ++i){
        int length = i * 9 + 100;
        int lst1[length];
        int lst2[length];
        for(int j = 0; j < length; ++j)
            lst1[j] = rand()%10000;

        for(int j = 0; j < length; ++j)
            lst2[j] = lst1[j];
        auto from = std::chrono::system_clock::now();
        mergeSort(lst2, length);
        auto to = std::chrono::system_clock::now();
        mergeTime = (to - from);
        
        for(int j = 0; j < length; ++j)
            lst2[j] = lst1[j];
        from = std::chrono::system_clock::now();
        qsort(lst2, length);
        to = std::chrono::system_clock::now();
        qTime = (to - from);
        
        for(int j = 0; j < length; ++j)
            lst2[j] = lst1[j];
        from = std::chrono::system_clock::now();
        heapSort(lst2, length);
        to = std::chrono::system_clock::now();
        heapTime = (to - from);
        qDat << length << " " << qTime.count()     << "\n";
        hDat << length << " " << heapTime.count()  << "\n";
        mDat << length << " " << mergeTime.count() << "\n";
    }
    qDat.close();
    mDat.close();
    hDat.close();
}
```

One minor result is that worst-case of qsort is awefull enough to make interests.
(Worst-cases for qsort was a list with elements with the same value)

<div align="center" id="sortResultPlot2">
</div>

<script>
    function readData(url, result){
        var thisText = new XMLHttpRequest(url);
        thisText.open("GET", url, false);
        thisText.setRequestHeader("Content-Type", "text");
        thisText.send(null);
        var body = thisText.responseText.split('\n');
        for(let i = 0; i < body.length; ++i){
            let line = body[i].trim();
            if(line != ''){
                let points = line.split(' ');
                result['x'].push(parseFloat(points[0]));
                result['y'].push(parseFloat(points[1]));
            }
        }
    }
    var heapRes = {name: 'Heap sort', type: 'scatter', x: [],y: []};
    var mergeRes = {name: 'Merge sort', type: 'scatter', x: [],y: []};
    var qRes = {name: 'Quick sort(Average)', type: 'scatter', x: [],y: []};
    var qWorstRes = {name: 'Quick sort(Worst)', type: 'scatter', x: [],y: []};
    readData("https://banramlo.github.io/assets/post/2020-11-15-SortingAlgorithm/heapSort.dat", heapRes);
    readData("https://banramlo.github.io/assets/post/2020-11-15-SortingAlgorithm/mergeSort.dat", mergeRes);
    readData("https://banramlo.github.io/assets/post/2020-11-15-SortingAlgorithm/qSort.dat", qRes);
    readData("https://banramlo.github.io/assets/post/2020-11-15-SortingAlgorithm/qSortWorst.dat", qWorstRes);
    var data = [heapRes, mergeRes, qRes, qWorstRes];
    var layout = {
        xaxis: {
            title: '# elements',
            showgrid: false,
            zeroline: false
        },
        yaxis: {
            title: 'Execution time(sec)',
            type: 'log',
            showline: false
        }
    };
    Plotly.newPlot('sortResultPlot2', data, layout);
</script>

