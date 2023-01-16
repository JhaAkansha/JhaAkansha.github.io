---
title: "Bubble Sort"
date: 2022-07-30T12:27:42+05:30
#draft: true
---
***Runtime***: &nbsp; O(n²) &emsp; &emsp; &emsp; ***Memory*** : &nbsp; O(1)

*\*In this article each n refers to the number of elements present in the array.*

In BubbleSort, we start at the beginning of the array and swap the first two elements if the first is greater than the second. Then, we go to the next pair and so on, continuously making sweeps of the array until it is sorted.  
It ia a stable algorithm, i.e, the relative positions of equivalent elements remains the same.  
 >**Implementation of simple BubbleSort**  
![img](/Pictures/simple_bubblesort.png "Implemetation of simple BubbleSort")

The above method is not optimized it could be bettered by stopping the algorithm if the inner loop does not cause any swap.  
The optimized approach will run a little slower than the original one if all the passes are made. However, in the best situation, the time complexity will be O(n) as opposed to the original which was (n²) in all circumstances.  

**Worst Case** : &nbsp; The worst case occurs when all the elements are arranged in descending order.  
Total number of iterations = n - 1  
At pass 1: &nbsp; comparisons = n - 1 &emsp; &emsp; &emsp; swaps = n - 1  
At pass 2: &nbsp; compartisons = n - 2 &emsp; &emsp; &emsp; swaps = n - 2  
and so on until the number of comparisons is 1. Therefore, total number of comparisons required to sort the array  
= (n - 1) + (n - 2) + (n - 3) + .... + 2 + 1  
= (n - 1)(n - 1 + 1)/2  
= n(n - 1)/2  
This is equivalent to n².  
**Best Case** : &nbsp; The best occurs when the array is already sorted. The time complexity in this case is O(n).  
 >**Implementation of optimized BubbleSort**  
![img](/Pictures/optimized_bubblesort.png "implementation of optimized BubbleSort")  

**RECURSIVE BUBBLESORT**  
The following steps are followed to implement recursive bubblesort:-  
1. Place the largest element at its position, this operation makes sure that the first largest element will be placed at the end of the array.
2. Recursively call for the rest n - 1 elements with the same operation and place the next greater element at its position.
3. The base condition for this recursion call would be when the number of elements in the array becomes 0 or 1 then, simply return.  
 >**Implementation of recursive BubbleSort**  
 ![img](/Pictures/recursive_bubblesort.png "implementation of recursive BubbleSort")