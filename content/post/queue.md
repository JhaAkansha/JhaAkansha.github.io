---
title: "Queue"
date: 2022-11-27T13:23:35+05:30
#draft: true
---
A queue is an ordered list in which insertions are done at one end (rear) and deletions are done at the other end (front). The first element to be inserted is the first element to be deleted, i.e. it follows FIFO (First In First Out) concept. Circular linked lists and circular arrays are used to implement queues.
## Main queue operations
* Enqueue: Inserts an element at the end of the queue.
* Dequeue: Removes and return the element at the front of the queue.
## Auxiliary queue operations
* Front: Returns the element at the front without removing it.
* QueueSize(): Returns the number of elements stored.
* IsEmptyQueue: Indicates whether no elements are stored.  
## Simple Circular Array Implementation
Elements are added circularly and two variables are used to keep track of the start element (front) and the element at the end (rear).  
The array may become full and an enqueue operation will throw a full queue exception. Similarly, deleting an element will throw an empty queue exception.
```C
struct arrayQueue {
    int front, rear;
    int capacity;
    int *array;
};
struct arrayQueue *Queue(int size) {
    struct arrayQueue *Q = malloc(sizeof(structarrayQueue));
    if (!Q) {
        return NULL;
    }
    Q->capacity = size;
    Q->front = Q->rear = -1;
    Q->array = malloc(Q->capacity*sizeof(int));
    if (!Q->array) {
        return NULL;
    }
    return Q;
}
int isEmptyQueue(struct arrayQueue *Q) {
    return (Q->front == -1);
}
int isFullQueue(struct arrayQueue *Q) {
    return( ((Q->rear+1) % (Q->capacity)) == Q->front);
}
int queueSize() {
    return ((Q->capacity - Q->front + Q->rear +1) % Q->capacity) ;
}
void Enqueue(struct arrayQueue *Q, int data) {
    if (isFullQueue(Q)) {
        printf("Queue Overflow");
    }
    else {
        Q->rear = (Q->rear +1)% Q->capacity;
        Q->array[Q->rear] = data;
        if (Q->front == -1) {
            Q->front = Q->rear;
        }
    }
}
int Dequeue(struct arrayQueue *Q) {
    int data = 0;
    if (isEmptyQueue(Q)) {
        printf("Queue is empty");
        return 0;
    }
    else {
        data = Q->array[Q->front];
        if (Q->front = Q->rear) {
            Q->front = Q->rear = -1;
        }
        else {
            Q->front = (Q->front+1) % Q->capacity;
        }
    }
    return data;
}
void deleteQueue(struct arrayQueue *Q) {
    if (Q) {
        if (Q->array) {
            free(Q->array);
        }
        free(Q);
    }
}
```
### Limitations
The maximum size of queue must be defined at the beginning and cannot be changed. Trying to enqueue a new element into a full queue causes an implementation- specific exception.
## Dynamic Circular Array Implementation
In the dynamic array implemntation of queue, the size of queue gets doubled in size each time the enqueue operation is called on a full queue.
```C
struct DynArrayQueue {
    int front, rear;
    int capacity;
    int *array;
};
struct DynArrayQueue * createQueue() {
    struct DynArrayQueue *Q = malloc(sizeof(struct DynArrayQueue));
    if (!Q) {
        return NULL;
    }
    Q->capacity = 1;
    Q->front = Q->rear = -1;
    Q->array = malloc(Q->capacity*sizeof(int));
    if (!Q->array) {
        return NULL;
    }
    return Q;
}
int isEmptyQueue(struct DynArrayQueue *Q) {
    return (Q->front == -1);
}
int isFullQueue(struct DynArrayQueue *Q) {
    return ((Q->rear + 1) % Q-> capacity == Q-front);
}
int queueSize() {
    return((Q->capacity - Q->front + Q->rear + 1) % Q->capacity);
}
void Enqueue(struct DynArrayQueue *Q, int data) {
    if(isFullQueue(Q)) {
        resizeQueue(Q);
    }
}
void resizeQueue(struct DynArrayQueue *Q) {
    int size = Q->capacity;
    Q->capacity = Q->capacity*2;
    Q->array = realloc(Q->array, Q->capacity);
    if (!q->array) {
        printf("Memory error");
        return;
    }
    if (Q->front > Q->rear) {
        for (int i = 0; i<Q->front; i++) {
            Q->array[i+size] = Q->array[i];
        }
        Q->rear = Q->rear + size;
    }
}
int Dequeue(struct DynArrayQueue *Q) {
    int data = 0;
    if (isEmptyQueue(Q)) {
        printf("Queue is empty");
        return 0;
    }
    else {
        data = Q->array[Q->front];
        if (Q->front == Q->rear) {
            Q->front = Q->rear = -1;
        }
        else {
            Q->front = (Q->front + 1) % Q->capacity;
        }
    }
    return data;
}
void deleteQueue(struct DynArrayQueue *Q) {
    if (Q) {
        if (Q->array) {
            free(Q->array);
        }
        free(Q->array) //CROSS CHECK THIS ONCE
    }
}
```
## LInked List Implementation
```C
struct ListNode {
    int data;
    struct ListNode *next;
};
struct queue {
    struct ListNode *front;
    struct ListNode *rear;
};
struct queue *createQueue() {
    struct queue *Q;
    struct ListNode *temp;
    Q = malloc(sizeof(struct queue));
    if (!Q) {
        return NULL;
    }
    temp = malloc(sizeof(struct ListNode));
    Q->front = Q->rear = NULL;
    return Q;
}
    int isEmptyQueue (struct queue *Q) {
        return (Q->front == NULL);
}
void Enqueue (struct queue *Q, int data) {
    struct ListNode *newNode;
    newNode = malloc(sizeof(struct ListNode));
    if (!newNode) {
        return NULL;
    }
    newNode->data = data;
    newNode->next = NULL;
    Q->rear->next = newNode;
    Q->rear = newNode;
    if (Q->front == NULL) {
        Q->front = Q->rear;
    }
}
int Dequeue(struct queue *Q) {
    int data = 0;
    struct ListNode *temp;
    if (isEmptyQueue(Q)) {
        printf("Queue is empty");
        return 0;
    }
    else {
        temp = Q->front;
        data = Q->front->data;
        Q->front = Q->front->next;
        free(temp);
    }
    return data;
}
void deleteQueue(struct queue *Q) {
    struct ListNode *temp;
    while (Q) {
        temp = Q;
        Q = Q->next;
        free(temp);
    }
    free(Q);
}
```