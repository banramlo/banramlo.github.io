---
layout: post
title: ABA problem
gh-repo: daattali/beautiful-jekyll
tags: [concurrency, problem]
comments: true
use_math: true
---

ABA is a well-known problem at CAS based lock-free data structure.
C++ supports std::atomic for abitrary data type but infact, it is sometimes truly lock-free and sometimes it isn't.
You can check it with below.

```c
#include <atomic>
#include <iostream>

typedef struct _bigStructure{
    int a;
    int b;
    float c;
    char d;
}bigStructure;

int main(){
    std::atomic<int> atomicInt;
    std::atomic<bigStructure> atomicStructure;

    std::cout << "std::atomic<int> atomicInt                  is it lock free? : " << atomicInt.is_lock_free() << std::endl;
    std::cout << "std::atomic<bigStructure> atomicStructure   is it lock free? : " << atomicStructure.is_lock_free() << std::endl;
    return 0;
}
```

Result will be looks like.

```c
std::atomic<int> atomicInt                  is it lock free? : 1
std::atomic<bigStructure> atomicStructure   is it lock free? : 0
```

In fact, CAS-possible data size is limited to the HW support.
However, most of HW supports only memory size CAS operation.
If it is 32-bit computing unit, it usually supports 32-bit CAS.
If it is 64-bit computing unit, it usually supports 64-bit CAS.
Therefore, most of CAS uses pointer to work.
Below lock-free queue uses this.

```c
#include <iostream>
#include <thread>
#include <string>
#include <atomic>
#include <stdio.h>
#include <unistd.h>
#include <mutex>

template<typename T>
int __coutArgs__(T t){
    std::cout << t;
}

template<typename T, typename ... Args>
int __coutArgs__(T t, Args... args){
    std::cout << t;
    __coutArgs__(args...);
}

std::mutex __print_sync_mutex__;

//Synchronized printing for cpp
template<typename ... Args>
int printSync(Args... args){
    __print_sync_mutex__.lock();
    __coutArgs__(args...);
    __print_sync_mutex__.unlock();
}

//List structure to save some profile
class profile_t{
public:
    profile_t(std::string name, std::string phoneNumber, profile_t* next):name(name),phoneNumber(phoneNumber),next(next){}
    std::string name;
    std::string phoneNumber;
    profile_t* next;
};

class profileStack_t{
public:
    std::atomic<profile_t*> top_ptr;
    profileStack_t(){
        top_ptr = nullptr;
    }

    profile_t* pop(){
        while (true) {
            profile_t* ret_ptr = top_ptr;
            if (ret_ptr == nullptr) return nullptr;//Empty stack
            profile_t* next_ptr = ret_ptr->next;
            if (top_ptr.compare_exchange_weak(ret_ptr, next_ptr)) {
                return ret_ptr;
            }
        }
    }

    profile_t* slowPop(){
        while (true) {
            profile_t* ret_ptr = top_ptr;
            if (ret_ptr == nullptr) return nullptr;//Empty stack
            profile_t* next_ptr = ret_ptr->next;
            printSync("T2 sleep with top pointer at ", ret_ptr, "\n");
            printSync("This CAS must failed if T1 access\n");
            sleep(2);
            if (top_ptr.compare_exchange_weak(ret_ptr, next_ptr)) {
                printSync("T2 Success\n");
                return ret_ptr;
            }
            else{
                printSync("T2 Failed\n");
            }
        }
    }

    void push(profile_t* obj_ptr){
        while (true) {
            profile_t* next_ptr = top_ptr;
            obj_ptr->next = next_ptr;

            if (top_ptr.compare_exchange_weak(next_ptr, obj_ptr)) {
                return;
            }
        }
    }
};

int main(){
    using namespace std;
    profile_t* A    = new profile_t("A", "010-1234-5678", nullptr);
    profile_t* B    = new profile_t("B", "010-1111-1111", nullptr);
    profileStack_t q;

    q.push(B);
    q.push(A);
    
    auto t1 = thread([&](){
        sleep(1);//Let t2 gets pointer first.
        auto popped1 = q.pop();
        if(popped1 != nullptr){
            printSync("T1 : pop ", popped1->name.c_str(), "(", popped1->phoneNumber, ") at ", popped1, "\n");
            delete popped1;
        }
        
        profile_t* newA    = new profile_t("newA", "010-0000-0000", nullptr); //Produce after delete A for using the same address
        
        auto popped2 = q.pop();
        if(popped2 != nullptr){
            printSync("T1 : pop ", popped2->name.c_str(), "(", popped2->phoneNumber, ") at ", popped2, "\n");
            delete popped2;
        }
        q.push(newA);
        printSync("T1 : push ", newA->name.c_str(), "(", newA->phoneNumber, ") at ", newA, "\n");
    });
    
    auto t2 = thread([&](){
        auto popped = q.slowPop();
        if(popped != nullptr){
            printSync("T2 : pop ", popped->name.c_str(), "(", popped->phoneNumber, ") at ", popped, "\n");
            delete popped;
        }
    });

    t1.join();
    t2.join();

    profile_t* popped;
    while(true){
        popped = q.pop();
        if(popped == nullptr)
            break;
        cout << "top : " << popped->name.c_str() << "(" << popped->phoneNumber <<") at " << popped << "\n";
        delete popped;
    }

}
```

However, it has a problem(You can try this in own).
Since it checks only the pointer to decide whether it changed or not,
it can't know when it just reuses the same address.
Therefore, above scenario will be a problem.

Let's assume there are one CAS-based lock-free queue $Q$ and two threads $T_1, T_2$.
$Q$ has $A$ and $B$ at the initial state.
Now $T_1$ access $Q$, it caches $A$ as top of stack and $B$ as next of the top.
However $T_1$ fall a sleep right after.
Now $T_2$ access $Q$, it pop $A$ and $B$. After that $T_2$ reuses $A$'s address and push it to the $Q$.
Now $T_1$ wakes up, $T_1$'s CAS will be success because it checks only top of the stack.
Therefore, top of stack will be a dangling reference to the $B$.
Now it is totally massed.