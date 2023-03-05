---
title: "Indexed Sequential Search"
date: 2022-09-11T04:55:43+05:30
#draft: true
---
In this searching method, first of all, an index file is created that contains some specific group or division of required record when the index is obtained, then the partial indexing takes less time because it is located in a specific group.  
## Characteristics of indexed sequential search
1. In indexed sequential search, a sorted index is set aside in addition to the array.
2. Each element in the index points to a block of elements in the array or another expanded index.
3. The index is searched first then the array and guides the search in the array.  
## Implementation of Indexed Sequential Search 
```C
int gn = 3; //gn is the number of elements in a group
int elements[gn], indices[gn], i, set =0;
int j = 0, ind = 0, start, end;
for (i = 0; i < n; i+=3) {
    elements[ind] = arr[i];
    indices[ind] = i;
    ind++;
}
if (k < elements[0]) {
    printf("Not found");
    exit(0);
}
else {
    for (i = 1; i <= ind; i++) {
        if (k <= elements[i]) {
            start = indices[i-1];
            end = indices[i];
            set = 1;
            break;
        }
    }
    if (set == 0) {
        start = indices[gn - 1];
        end = gn;
    }
    for (i = start; i <= end; i++) {
        if (k == arr[i]) {
            j = 1;
            break;
        }
    }
}
    if (j == 1) {
        printf("Found at index %d", i);
    }
    else {
        printf("element not found");
    }
```