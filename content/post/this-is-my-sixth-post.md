---
title: "Merge Sort"
date: 2022-09-17T13:18:43+05:30
#draft: true
---
***Runtime***: &nbsp; O(nlogn) &emsp; &emsp; &emsp; ***Auxiliary Space*** : &nbsp; O(n)

Merge Sort divides the array in half, sorts each of those halves and then merges them back together. Each of these halves has the same sorting algorithm applied to it. Eventually, its like merging two single-element arrays. The merge method operates by copying all the elements from target array segment into a helper array,keeping track of where the start of the left and right halves should be. Then, iterate through helper, copying the smaller element from each half into the array.  
Its is a stable algorithm.  
>**Implemetation of MergeSort**  
![img](/Pictures/mergesort.png "Implemetation of MergeSort")  

**DRAWBACKS**  
1. It is slower compared to other sort algorithms for smaller tasks.
2. It requires additional memory space of O(n).
3. Merge sort goes through whole process even if array is sorted.