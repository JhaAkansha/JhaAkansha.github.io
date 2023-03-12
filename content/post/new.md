---
title: "Stack"
date: 2022-11-05T16:23:59+05:30
#draft: true
---

A stack is an ordered list in which insertion and deletion are done at one end called the "top".  
## Main Stack Operations
* Push: Inserts data onto the stack.  
* Pull: Removes and returns the last inserted data from the stack.  
## Auxiliary Stack Operations
* Top: Returns the last inserted element without removing it.  
* Size: returns  the number of elements stored in the stack.  
* IsEmptyStack: Check whether the stack is empty or not.  
* IsFullStack: Check whether the stack is full or not.  
## Simple Array Implementation
This implementation of stack uses simple array. We add elements from left to right and use a variable to keep track of the index of the top element. If the array i full, a push operation will throw a *full stack exception*. Similarly, if we try to delete an element from an empty array, it will throw a *stack empty exception*.  
```C
struct StackArr { 
	int top;
	int capacity;
	int *array;
};
struct StackArr *CreateStack() {
	struct StackArr *S = malloc(sizeof(struct StackArr));
	if (!S) {
		return NULL;
	}
	S->capacity = 1;
	S->top = -1;
	S->array = malloc(S->capacity*sizeof(int));
	if(!S->array) {
		return NULL;
	}
	return S;
}
int IsEmptyStack (struct StackArr *S) {
	return (S->top == -1);
}
int IsFullStack (struct StackArr *S) {
	return (S->top == S->capacity - 1);
}
void Push (struct StackArr *S, int data) {
	if (IsFullStack(S)) {
		printf("Stack Overflow");
	}
	else {
		S->top++;
		S->array[S->top]=data;
	}
}
int Pop (struct StackArr *S) {
	if (IsEMptyStack(S)) {
		printf("Stack is empty");
		return 0;
	}
	else {
		return (S->array[top--]);
	}
}
```
### Performance 
| Function | Time complexity |  
| -----------------------------------------| -----|  
| Space Complexity (for n push operations) | O(n) |  
| Time Complexity of Push()                | O(1) |  
| Time Complexity of Pop()                 | O(1) |  
| Time Complexity of Size()                | O(1) |  
| Time Complexity of IsEmptyStack()        | O(1) |  
| Time Complexity of IsFullStack()         | O(1) |  

However, the maximum size of the array must be defined at the beginning and cannot be changed. Trying to push a new element into a full stack causes an implementation-specific exception. Therefore, a simple array implementation is not ideal and hence dynamic array implementation is preferred.  
## Dynamic Array Implementation
In this approach, if the array is full, we create a new array of twice the size and copy items. With this approach, pushing n items takes time proportional to n.  

```C
struct DynArr {
	int top;
	int capacity;
	int *array;
};
struct DynArr *CreateStack() {
	struct DynArr *S = malloc(sizeof(struct DynArr));
	if(!S) {
		return NULL;
	}
	else {
		S->capacity = 1;
		S->top = -1;
		S->array = malloc(S->capacity*sizeof(int));
	}
	if (S->array) {
		return NULL;
	}
	return S;
}
int IsFullStack(struct DynArr *S) {
	return (S->top == S->capacity-1);
}
void DoubleStack(struct DynArr *S) {
	S->capacity*=2;
	S->array = realloc(S->array, S->capacity);
}
void Push(struct DynArr *S, int x) {
	if (IsFullStack(S)) {
		DoubleStack(S);
	}
	S->array[++S->top] = x;
}
int IsEmptyStack(struct DynArr *S) {
	return S->top == -1;
}
int Top(struct DynArr *S) {
	if (IsEmptySrack(S)) {
		return INT_MIN;
	}
	return S->array[S->top];
}
int Pop(struct DynArr *S) {
	if(IsEmptyStack(S)) {
		return INT_MIN;
	}
	return S->array[S->top--];
}
```
### Perfromance  
| Function | Time complexity |  
| -----------------------------------------| -----|  
| Space Complexity (for n push operations) | O(n) |  
| Time Complexity of Push()                | O(1) (Average)|  
| Time Complexity of Pop()                 | O(1) |  
| Time Complexity of Size()                | O(1) |  
| Time Complexity of IsEmptyStack()        | O(1) |  
| Time Complexity of IsFullStack()         | O(1) |  

The limitation of this implementation of stack is that too many doublings may cause memory overflow exception.  
## Linked List Implementation
Another way of implementing stacks is by using linked lists. Push operation is implemented by inserting the incoming element at the beginning of the list. Pop operation  is implemented by deleting the node from the beginning.  

```C
struct ListNode {
	int data;
	struct ListNode *next;
};
struct Stack *CreateStack() {
	return NULL;
}
void Push(struct Stack **top, int data) {
	struct Stack *temp;
	temp = malloc(sizeof(struct Stack));
	if (!temp) {
		return NULL;
	}
	temp->data = data;
	temp->next = *top;
	*top = temp;
}
int IsEmptyStack(struct Stack *top) {
	return top == NULL;
}
int Pop(struct Stack **top) {
	int data;
	struct Stack *temp;
	if (IsEmptyStack(top)) {
		return INT_MIN;
	}
	temp = *top;
	*top = *top->next;
	data = temp->data;
	free(temp);
	return data;
}
int Top(struct Stack *top) {
	if (IsEmptyStack(top)) {
		return INT_MIN;
	}
	return top->next->data;
}
```

### Performance  
| Function | Time complexity |  
| -----------------------------------------| -----|  
| Space Complexity (for n push operations) | O(n) |  
| Time Complexity of Push()                | O(1) (Average)|  
| Time Complexity of Pop()                 | O(1) |  
| Time Complexity of Size()                | O(1) |  
| Time Complexity of IsEmptyStack()        | O(1) |  
| Time Complexity of IsFullStack()         | O(1) |  


>## Comparing Array Implementation and Stack Implementation
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
