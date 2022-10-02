---
title: "Binary Search"
date: 2022-09-22T04:35:32+05:30
#draft: true
---
***Runtime*** : &nbsp; O(log n)  
The approximate middle item of the data set is located, and its key value is examined. If its value is too high, then the key of the middle element of the first half of the set is examined and procedure is repeated on the first half until the required item is found. If the value is too low, then the key of the middle entry of the second half of the data set is tried and the procedure is repeated on the second half. The process is continued until the desired key is found or search interval becomes empty. 
>**Implementation of Binary search**  
![img](/Pictures/binarysearch.png "Implementation of binary search")  

**RECURSIVE BINARY SEARCH**  
Binary search can be implemented using recursion as well.  
>**Implementation of recursive Binary search**  
![img](/Pictures/recurbinary.png "Implementation of heap sort")  
