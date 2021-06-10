---
layout: post
title: CUDA
gh-repo: daattali/beautiful-jekyll
tags: [algorithm, concurrency, graph]
comments: true
use_math: true
---

CUDA is a parallel computing model, architectural API model for GPU.

It supports multiple abstractions to handle NVIDIA GPU.

This post will be updated later.

### ​cudaError_t cudaMalloc ( void** devPtr, size_t size ) ###

This instruction allocates some memory space to GPU with size.
It will return address of memory space to devPtr.
If it fails, it will return some values as the return of function.

### cudaError_t cudaFree ( void* devPtr ) ###

This instruction deallocates memory in devPtr on GPU.
If it fails, it will return some values as the return of function.
    
### cudaError_t cudaMemcpy ( void* dst, const void* src, size_t count, cudaMemcpyKind kind ) ###

This instruction copies memory from CPU memory to GPU memory or between them.
It copies memory from src to dst with size of count.
It has 5 kinds of data transfer and following is that.
1. cudaMemcpyHostToHost : Copy memory between CPU memory space.
2. cudaMemcpyHostToDevice : Copy CPU memory space to GPU memory space.
3. cudaMemcpyDeviceToHost : Copy GPU memory space to CPU memory space.
4. cudaMemcpyDeviceToDevice : Copy memory between GPU memory space.
5. cudaMemcpyDefault : Automatically transfer the data. It requires to be unified memory.



### kernel call ###

To run the kernel of GPU, you need to use it like below.
Notice that kernel call passes three parameters.

```cpp
nameOfKernelCall<<<Number of Block, Number of Thread>>>(parameters);
```

### Example ###

With the function above, it needs some kernel function to be used.
Example code is like below.

```cpp
#include <iostream>
#include <time.h>

__global__ void _vectorAdd(int* data1, int* data2, int* result, int n){
    int globalIdx = threadIdx.x + blockIdx.x * blockDim.x;
    if(globalIdx >= n)//Thread index overflow
        return;

    //Vector addition
    result[globalIdx] = data1[globalIdx] + data2[globalIdx];
}

void vectorAdd(int *data1, int *data2, int* result, int n){
    //Memory spaces in the GPU device
    int *dataAtGpu1, *dataAtGpu2, *resultAtGpu;
    cudaMalloc(&dataAtGpu1, sizeof(int) * n);
    cudaMalloc(&dataAtGpu2, sizeof(int) * n);
    cudaMalloc(&resultAtGpu, sizeof(int) * n);

    //Memory transfer from CPU to GPU
    cudaMemcpy(dataAtGpu1, data1, sizeof(int) * n, cudaMemcpyHostToDevice);
    cudaMemcpy(dataAtGpu2, data2, sizeof(int) * n, cudaMemcpyHostToDevice);

    //Kernel call
    _vectorAdd<<<(n + 1023)/1024, n%1024>>>(dataAtGpu1, dataAtGpu2, resultAtGpu, n);

    //Result transfer from GPU to CPU
    cudaMemcpy(result, resultAtGpu, sizeof(int) * n, cudaMemcpyDeviceToHost);
}

int main(){
    srand(time(NULL));
    constexpr int numVector = 1000;
    int data1[numVector], data2[numVector];
    for(int i = 0; i < numVector; ++i){//Initialize vector1, vector2 to some random values
        data1[i] = rand() % 100;
        data2[i] = rand() % 100;
    }
    int resCPU[numVector];//Result of the vector addition on CPU
    for(int i = 0; i < numVector; ++i){
        resCPU[i] = data1[i] + data2[i];//CPU type of vector addition
    }
    
    int resGPU[numVector];//Result of the vector addition on CGPU
    vectorAdd(data1, data2, resGPU, numVector);//GPU(CUDA) type of vector addition

    for(int i = 0; i < numVector; ++i){
        if(resGPU[i] != resCPU[i]){//Error handler
            printf("Wrong result\n");
            return 0;
        }
    }
    printf("Test passed\n");
    return 0;
}
```


Notice that this can be run in parallel with CPU at the same time.
