---
title: "Linked List"
date: 2022-10-02T19:17:59+05:30
#draft: true
---
A linked list is a list or chain of items where each item points to the next one in the list. Each item in a linked list is called a node. Each node contains the data and the location of the next item.  

***PROPERTIES***
* Successive elements are connected by pointers.
* Last element points to NULL.
* Can grow or shrink in size during execution of a program.
* Can be made just as long as required.
* It does not waste memory space (but takes some extra memory for pointers). 
***Disadvantages***
* Large access time to individual element.
* An advantage of arrays in access time is special locality in memory. Arrays are defined as contiguous blocks of memory, and so any array element will be physically near its neighbours. This greatly benefits from modern CPU caching methods.
* Linked lists can be hard to manipulate.
* Waste memory in terms of extra reference points.  


**SINGLY LINKED LIST**  
***Type declaration***  
struct listNode {  
 &emsp;int data;  
&emsp;struct listNode *next;  
}  
***Traversing the list (Time: O(n) &emsp; Space: O(1))***  
int listLength (struct listNode *head) {  
&emsp;struct listNode *current = head;  
&emsp;int count= 0;  
&emsp;while (currnet != NULL) {  
&emsp;&emsp;count++;  
&emsp;&emsp;current = current->next;  
&emsp;}  
&emsp;return count;  
}  
***Inserting an element (Time: O(n) &emsp; Space: o(1))***  
void Insert (struct listNode **head, int data, int position) {  
&emsp;int k = 1;  
&emsp;struct listNode *p, *q, *newNode;  
&emsp;newNode = (listNode*)malloc(sizeof(struct listNode));  
&emsp;if (!newNode) {  
&emsp;&emsp;printf("Memory Error");  
&emsp;&emsp;return;  
&emsp;}  
&emsp;newNode->data = data;  
&emsp;p = *head;  
&emsp;if (position == 1) {  
&emsp;&emsp;newNode->next = p;  
&emsp;&emsp;*head = newNode;  
&emsp;}  
&emsp;else {  
&emsp;&emsp;while ((p != NULL) && (k < position - 1)) {  
&emsp;&emsp;&emsp;k++;  
&emsp;&emsp;&emsp;q = p;  
&emsp;&emsp;&emsp;p = p->next;  
&emsp;&emsp;}  
&emsp;&emsp;if (p == NULL) {  
&emsp;&emsp;&emsp;q->next = newNode;  
&emsp;&emsp;&emsp;newNode->next = NULL;  
&emsp;&emsp;}  
&emsp;&emsp;else {  
&emsp;&emsp;&emsp;q->next = newNode;  
&emsp;&emsp;&emsp;newNode->next = p;  
&emsp;&emsp;}  
&emsp;}  
}  
***Deleting a node***  
void deleteNode (struct listNode **head, int position)  {  
&emsp;    int k = 1;  
&emsp;    struct listNode *p, *q;  
&emsp;    if (*head == NULL) {  
&emsp;&emsp;        printf("List Empty");  
&emsp;&emsp;        return;  
&emsp;    }  
&emsp;    p = *head;  
&emsp;    if (position == 1) {  
&emsp;&emsp;        p = *head;  
&emsp;&emsp;        *head = *head->next;  
&emsp;&emsp;        free(p);  
&emsp;&emsp;        return;  
&emsp;    }  
&emsp;    else {  
&emsp;&emsp;        while ((p != NULL) && (k < position - 1)) {  
&emsp;&emsp;&emsp;            k++;  
&emsp;&emsp;&emsp;            q = p;  
&emsp;&emsp;&emsp;            p = p->next;  
&emsp;&emsp;        }  
&emsp;&emsp;        if (p == NULL)  {  
&emsp;&emsp;&emsp;            printf("Position does not exist");  
&emsp;&emsp;        }  
&emsp;&emsp;        else {  
&emsp;&emsp;&emsp;            q->next = p->next;  
&emsp;&emsp;&emsp;            free(p);  
&emsp;&emsp;        }  
&emsp;    }  
}  