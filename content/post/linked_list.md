---
title: "Linked List"
date: 2022-10-21T19:17:59+05:30
#draft: true
---
A linked list is a list or chain of items where each item points to the next one in the list. Each item in a linked list is called a node. Each node contains the data and the location of the next item.  

## PROPERTIES
* Successive elements are connected by pointers.
* Last element points to NULL.
* Can grow or shrink in size during execution of a program.
* Can be made just as long as required.
* It does not waste memory space (but takes some extra memory for pointers). 
### Disadvantages
* Large access time to individual element.
* An advantage of arrays in access time is special locality in memory. Arrays are defined as contiguous blocks of memory, and so any array element will be physically near its neighbours. This greatly benefits from modern CPU caching methods.
* Linked lists can be hard to manipulate.
* Waste memory in terms of extra reference points.  


## SINGLY LINKED LIST  
### Type declaration
```C
struct listNode {  
int data;  
struct listNode *next;  
}
```
### Traversing the list  
Time: O(n) &emsp; Space: O(1)
```C
int listLength (struct listNode *head) {  
    struct listNode *current = head;  
    int count= 0;  
    while (currnet != NULL) {  
        count++;  
        current = current->next;  
    }  
    return count;  
}
```

### Inserting an element  
Time: O(n) &emsp; Space: o(1)
```C
void insert(struct listNode **head, int data, int position) {  
    int k = 1;  
    struct listNode *p, *q, *newNode;  
    newNode = (listNode*)malloc(sizeof(struct listNode));  
    if (!newNode) {  
        printf("Memory Error");  
        return;  
    }  
    newNode->data = data;  
    p = *head;  
    if (position == 1) {  
        newNode->next = p;  
        *head = newNode;  
    }  
    else {  
        while ((p != NULL) && (k < position - 1)) {  
            k++;  
            q = p;  
            p = p->next;  
        }  
        if (p == NULL) {  
            q->next = newNode;  
            newNode->next = NULL;  
        }  
        else {  
            q->next = newNode;  
            newNode->next = p;  
        }  
    }  
}
```
### Deleting a node
```C  
void deleteNode (struct listNode **head, int position)  {  
    int k = 1;  
    struct listNode *p, *q;  
    if (*head == NULL) {  
        printf("List Empty");  
        return;  
    }  
    p = *head;  
    if (position == 1) {  
        p = *head;  
        *head = *head->next;  
        free(p);  
        return;  
    }  
    else {  
        while ((p != NULL) && (k < position - 1)) {  
            k++;  
            q = p;  
            p = p->next;  
        }  
        if (p == NULL)  {  
            printf("Position does not exist");  
        }  
        else {  
            q->next = p->next;  
            free(p);  
        }  
    }  
}  
```
## DOUBLY LINKED LIST
In doubly linked lists, given anode, we can navigate the list in both directions.  
A node in a singly linked list cannot be removed unless we have the pointer to its predecessor. But in a doubly linked list, we can delete a node if we don't have previous nodes, address (since each node has left pointer pointing to a previous node and we can move backwards).  
### Disadvantages
* Each node requires an extra pointer. requiring more space.
* The insertion or deletion of a node takes a little longer.
### Type declaration
```C
struct DLLnode {
    int data;
    struct DLLnode *next;
    struct DLLnode *prev;
}
```

### Inserting an element
```C
void DLLInsert(struct DLLnode **head, int data, int position) {
    int k = 1;
    struct DLLnode *temp, *newNode;
    newNode = (struct DLLnode*)malloc(sizeof(struct DLLnode));
    if (!newNode) {
        printf("Memory error");
        return;
    }
    newNode->data = data;
    if (position == 1) { //insert at the beginning
        newNode->next = *head;
        newNode->prev = NULL;
        *head->prev = newNode;
        *head = newNode;
        return;
    }
    temp = *head;
    while( (k < position - 1) && temp->next != NULL) {
        temp = temp->next;
        k++;
    }
    if (temp->next == NULL) { //insert at the end
        newNode->next = temp->next;
        newNode->prev = temp;
        temp->next = newNode;
    }
    else { //in the middle
        newNode->next = temp->next;
        newNode->prev = temp;
        temp->next->prev = newNode;
        temp->next = newNode;
    }
    return;
}
```

### Deleting a node
```C
void DLLNode(struct DLLNode **head, int position) {
    struct DLLNode *temp, *temp2, temp = *head;
    int k = 1;
    if (*head == NULL) {
        printf("List is empty");
        return;
    }
    if (position == 1) {  //at the beginning
        *head = *head->next;
        if (*head != NULL) {
            *head->prev = NULL;
        }
        free(temp);
        return;
    }
    while ( (k<position - 1) &&  temp->next != NULL) {
        temp = temp->next;
        k++;
    }
    if (temp->next == NULL) { //from the end
        temp2 = temp->prev;
        temp2->next = NULL;
        free(temp);
    }
    else {  //in the middle
        temp2 = temp->prev;
        temp2->next = temp->next;
        temp->next->prev = temp2;
        free(temp);
    }
    return;
}
```

## CIRCULAR LINKED LIST
### Type declaration
```C
struct CLLnode {
    int data;
    struct CLLnode *next;
}
```

### Inserting a node at the end
```C
void insertAtEnd(struct CLLnode **head, int data) {
    struct CLLnode current = *head;
    struct CLLnode *newNode = (struct node*)malloc(sizeof(struct CLLnode));
    if (!newNode){
        printf("Memory Error");
        return;
    }
    newNode->data = data;
    while(current->next != *head) {
        current = cuurent->next;
    }
    newNode->next = newNode;
    if (*head == NULL) {
        *head = newNode;
    }
    else {
        newNode->next = *head;
        current->next = newNode;
    }
}
```

### Inserting a node at the front
```C
void insertAtBegin(struct CLLnode **head, int data) {
    struct CLLnode *current = *head;
    struct CLLnode *newNode = (struct node*)malloc(sizeof(struct CLLnode));
    if (!newNode) {
        printf("Memory Error");
        return;
    }
    newNode->data = data;
    while (current->next != *head) {
        current = current->next;
    }
    newNode->next = newNode;
    if (*head == NULL) {
        *head = newNode;
    }
    else {
        newNode->next = *head;
        current->next = newNode;
        *head = newNode;
    }
    return;
}
```

### Deleting a node at the front
```C
void deleteFront(struct CLLnode **head) {
    struct CLLnode *temp = *head;
    struct CLLnode *current = *head;
    if (*head ==  NULL) {
        printf("List is empty");
        return;
    }
    while (current->next != *head) {
        current = current->next;
    }
    current->next = *head->next;
    *head = *head->next;
    free(temp);
    return;
}
```

### Deleting a node from the end
```C
void deleteLast (struct CLLnode **head) {
    struct CLLNode *temp = *head;
    struct CLLnode *current = *head;
    if (*head == NULL) {
        printf("List is empty");
        return;
    } 
    while (current->next != *head) {
        temp = current;
        current = current->next;
    }
    free(current);
    return;
}
```