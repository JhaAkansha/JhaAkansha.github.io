---
title: "Merge Sort"
date: 2022-08-21T13:18:43+05:30
#draft: true
---
***Runtime***: &nbsp; O(nlogn) &emsp; &emsp; &emsp; ***Auxiliary Space*** : &nbsp; O(n)

Merge Sort divides the array in half, sorts each of those halves and then merges them back together. Each of these halves has the same sorting algorithm applied to it. Eventually, its like merging two single-element arrays. The merge method operates by copying all the elements from target array segment into a helper array,keeping track of where the start of the left and right halves should be. Then, iterate through helper, copying the smaller element from each half into the array.  
It is a stable algorithm.  
## Implementation of MergeSort
```C
void mergesort(int[] array) {
    int[] helper = new int[array.length];
    mergesort(array, helper, 0, array.length - 1);
}
void mergesort(int[] array, int[] helper, int low, int high) {
    if (low < high) {
        int middle = (low+high)/2;
        mergesort(array, helper, low, middle); //sort left half
        mergesort(array, helper, middle+1, high); //sort right half
        mergesort(array, helper, low, middle, high); //merge them
    }
}
void merge(int[] array, int[] helper, int low, int middle, int high) {
    //copy both halves into a helper array
    for (int i = low; i<=high; i++) {
        helper[i] = array[i];
    }
    int helperLeft = low;
    int helperRight = middle + 1;
    int current = low;
    while (helperLeft <= middle && helperRight <= high) {
        if (helper[helperLeft] <= helper[helperRight]) {
            array[current] = helper[helperLeft];
            helperLeft++;
        }
        else {
            array[current] = helper[helperRight];
            helperRight++;
        }
        current++;
    }
    int remaining = middle - helperLeft;
    for (int = 0; i <= remaining; i++) {
        array[current + i] = helper[helperLeft + i];
    }
}
```

## Drawbacks    
1. It is slower compared to other sort algorithms for smaller tasks.
2. It requires additional memory space of O(n).
3. Merge sort goes through whole process even if array is sorted.