---
title: "Indexed Sequential Search"
date: 2022-09-22T04:55:43+05:30
#draft: true
---
In this searching method, first of all, an index file is created that contains some specific group or division of required record when the index is obtained, then the partial indexing takes less time because it is located in a specific group.  
**Characteristics of indexed sequential search**  
1. In indexed sequential search, a sorted index is set aside in addition to the array.
2. Each element in the index points to a block of elements in the array or another expanded index.
3. The index is searched first then the array and guides the search in the array.  
>**Implementation of Indexed Sequential Search**  
![img](/Pictures/ndexedsequentialsearch.png "Implementation of heap sort")  
