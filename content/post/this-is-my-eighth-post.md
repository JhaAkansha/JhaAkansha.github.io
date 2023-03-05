---
title: "Heap Sort"
date: 2022-09-03T10:44:35+05:30
#draft: true
---
***Runtime***: &nbsp; O(nlogn) &emsp; &emsp; &emsp; ***Memory*** : &nbsp; O(1)

Heap Sort is a comparison-based sorting technique based on binary heap data structure. It is an in-place algorithm. Its typical implementation is not stable, but can be made stable. Typically, it is 2-3 times slower than well-implemented quicksort due to lack of locality of reference.  
## Advantages of heapsort
1. *Efficiency* : The time required to perform heap sort increases logarithmicallywhile other algorithms may grow exponentially slower as the number of items to sort increases.  
2. *Memory usage* : Memory usage is minimal because apart from what is necessary to hold the initial list of items to be sorted,it needs no additional memory space to work.  
3. *Simplicity* : It is simpler to understand than other sorting algorithm because it does not use advanced computer science concepts.  
## Heapify
It is the process of creating a heap data structure from a binary tree represented using an array. It is used to create Min-Heap or Max-Heap.Start from the first index of the non-leaf node whose index is given by n/2 - 1. Heapify using recursion.  
## Implementation of Heap Sort  
```C
void heapify (int arr[], int n, int i) {
    int largest = i;
    int left = 2*i + 1;
    int right = 2*i + 2;
    if (left < n && arr[left] > arr[largest]){
        largest = left;
    }
    if (right < n && arr[right] > arr[largest]) {
        largest = right;
    }
    if (largest != i) {
        swap(&arr[i],&arr[largest]);
        heapify(arr, n, largest);
    }
}
void heapSort(int arr[], int n) {
    for (int i = n/2 - 1; i >= 0; i--) {
        heapify(arr, n, i);
    }
    for (int i = n - 1; i >= 0; i--) {
        swap(&arr[0], &arr[i]);
        heapify(arr, i, 0);
    }
}
```

## What are the two phases of heap sort?  
1. Array is converted into max heap.  
2. Highest element is removed and the remaining elements are used to create a new max heap.  
* **Which is better: Heapsort or Mergesort?** 
Mergesort is slightly faster than heapsort but mergesort requires extra storage space. Depending on the requirement, one should be chosen.  
* **Why is heapsort better than selection sort?**  
Heapsort is similar to selection sort, but with a better way to get the maximum element. It takes advantage os the heap data structure to get the maximum element in constant time.