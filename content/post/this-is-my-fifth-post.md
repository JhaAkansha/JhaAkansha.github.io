---
title: "Insertion Sort"
date: 2022-09-17T02:03:01+05:30
#draft: true
---
***Best Case Runtime***: &nbsp; O(n) &emsp; &emsp; &emsp; ***Worst Case Runtime*** : &nbsp; O(n²)

The method for implementation of insertion sort is similar to the one we use to arrange our cards. It is a stable sorting algorithm. The steps followed are:  
* Iterate from arr[1] to arr[n] over the array.
* Compare the current element (key) to its predecessor.
If the key element is smaller than its predecessor, compare it to the element before. Move the greater elements one position up to make space for the swapped element.
>**Implementation of InsertionSort**  
![img](/Pictures/insertionsort.png "Implemetation of InsertionSort")  

**BINARY INSERTIONSORT**
This method uses binary search to find the proper location to insert the selected item at each iteration. In normal insertion, sorting takes O(i) time (at ith iteration) in worst case, this can be reduced to O(log i). The algorithm, as a whole, still has the worst case running time of O(n²).
>**Implementation of binary InsertionSort**  
![img](/Pictures/binaryinsertionsort.png "Implementation of Binary InsertionSort")