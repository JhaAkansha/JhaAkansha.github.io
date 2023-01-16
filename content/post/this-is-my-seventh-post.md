---
title: "Quick Sort"
date: 2022-08-26T13:18:51+05:30
#draft: true
---
***Runtime***: &nbsp; average : O(nlogn) &emsp; worst : O(n²) &emsp; &emsp; &emsp; ***Auxiliary Space*** : &nbsp; O(n)
In quick sort, we pick a random element and partition the array, such that all numbers that are less than the partitioning element come before all elements greater than it.   
If we repeatedly partition the array around an element, the array will eventually become sorted. However, as the partitioned element is not huaranteed to be the median, our sorting could be very slow.  
Quick Sort is not a stable algorithm. However, any sorting algorithm can be made stable by considering indexes as comparison parametres.  
>**Implementation of QuickSort**  
![img](/Pictures/quicksort.png "Implementation of quicksort")  

*Worst Case*  
The worst case occurs when the partition process always picks the greatest or the smallest element as the pivot. If we consider the partition startegy where the last element is always picked as a pivot, the worst case obviously occurs when the array is already sorted.  
*Best Case*  
The best case occurs when the partition process always picks the middle element as the pivot.  
**3 WAY QUICKSORT**  
Consider an array which has many redundant elements. For example {1,2,3,6,6,8,9,9,0,0,0,7}, If we pick 6 as the pivot, we fix only one 6 and recursively process remaining occurrences. The idea of 3 way quicksort is to process all occurrences of the pivot and is based on Dutch National Flag algorithm.  
>**Implementation of 3 way quicksort using Dutch National Flag algorithm**  
![img](/Pictures/dnfa_quicksort.png "Implementation of quicksort")  

The time complexity of this algorithm is O(nlogn) and the space complexity is o(logn).  
**Why is Quick Sort preferred over Merge Sort for sorting arrays?**  
* QuickSort is an in-place sort (i.e. it does not require any extra storage) whereas mergesort requires O(n) extra storage.
* Allocating and de-allocating the extra space used for mergesort increases the running time of the algorithm.
* Most practical implementations of QuickSort use randomized version. It has expected time complexity of O(nlogn). The worst case is possible in randomized version also, but worst case does not occur for a particular pattern (like sorted array) and works well in practice.
* QuickSort is also cache friendly sorting algorithm as it has good locality of reference when used for arrays.
* It is also tail recursive, therefore tail call optimization is done.  
