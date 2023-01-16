---
title: "Stack"
date: 2022-10-07T16:23:59+05:30
#draft: true
---

A stack is an ordered list in which insertion and deletion are done at one end called the "top".  
**Main Stack Operations**  
* Push: Inserts data onto the stack.  
* Pull: Removes and returns the last inserted data from the stack.  
**Auxiliary Stack Operations**  
* Top: Returns the last inserted element without removing it.  
* Size: returns  the number of elements stored in the stack.  
* IsEmptyStack: Check whether the stack is empty or not.  
* IsFullStack: Check whether the stack is full or not.  
>***Simple Array Implementation***  
This implementation of stack uses simple array. We add elements from left to right and use a variable to keep track of the index of the top element. If the array i full, a push operation will throw a *full stack exception*. Similarly, if we try to delete an element from an empty array, it will throw a *stack empty exception*.  
//code editor//  
**Performance**  
| Function | Time complexity |  
| -----------------------------------------| -----|  
| Space Complexity (for n push operations) | O(n) |  
| Time Complexity of Push()                | O(1) |  
| Time Complexity of Pop()                 | O(1) |  
| Time Complexity of Size()                | O(1) |  
| Time Complexity of IsEmptyStack()        | O(1) |  
| Time Complexity of IsFullStack()         | O(1) |  

However, the maximum size of the array must be defined at the beginning and cannot be changed. Trying to push a new element into a full stack causes an implementation-specific exception. Therefore, a simple array implementation is not ideal and hence dynamic array implementation is preferred.  
>***Dynamic Array Implementation***  
In this approach, if the array is full, we create a new array of twice the size and copy items. With this approach, pushing n items takes time proportional to n.  
//Code Editor//  
**Perfromance**  
| Function | Time complexity |  
| -----------------------------------------| -----|  
| Space Complexity (for n push operations) | O(n) |  
| Time Complexity of Push()                | O(1) (Average)|  
| Time Complexity of Pop()                 | O(1) |  
| Time Complexity of Size()                | O(1) |  
| Time Complexity of IsEmptyStack()        | O(1) |  
| Time Complexity of IsFullStack()         | O(1) |  

The limitation of this implementation of stack is that too many doublings may cause memory overflow exception.  
>***Linked List Implementation***  
Another way of implementing stacks is by using linked lists. Push operation is implemented by inserting the incoming element at the beginning of the list. Pop operation  is implemented by deleting the node from the beginning.  
//Code Editor//  
**Performance**  
| Function | Time complexity |  
| -----------------------------------------| -----|  
| Space Complexity (for n push operations) | O(n) |  
| Time Complexity of Push()                | O(1) (Average)|  
| Time Complexity of Pop()                 | O(1) |  
| Time Complexity of Size()                | O(1) |  
| Time Complexity of IsEmptyStack()        | O(1) |  
| Time Complexity of IsFullStack()         | O(1) |  

> ***Comparing Array Implementation and Stack Implementation***  
>
>**Array Implementation**
>* Operations take constant time.  
>* Expensive doubling operations every once in a while.  
>* Any sequence of n operations takes time proportional to n.  
>
>**Linked List Implementation**
>* Grows and shrinks gracefully.  
>* Every operation takes constant time O(1).  
>* Every operation uses extra time and space to deal with references.  
