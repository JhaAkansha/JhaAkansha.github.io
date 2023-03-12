---
title: "Binary Search"
date: 2022-10-07T04:35:32+05:30
---
***Runtime*** : &nbsp; O(log n)  
The approximate middle item of the data set is located, and its key value is examined. If its value is too high, then the key of the middle element of the first half of the set is examined and procedure is repeated on the first half until the required item is found. If the value is too low, then the key of the middle entry of the second half of the data set is tried and the procedure is repeated on the second half. The process is continued until the desired key is found or search interval becomes empty. 
## Implementation of Binary search
### Template 1
```C
int binarySearch (int a[], int n, int key) {
    int found = 0; mid, low = 0, high = n-1;
    while (low <= high) {
        mid = (low+high)/2;
        if (key < a[mid]) {
            high = mid - 1;
        }
        else if (key > a[mid]) {
            low = mid + 1;
        }
        else {
            found = 1;
            break;
        }
    }
    return found;
}
```

### Template 2
```C
int binary(int[] arr, int key) {
    int low = 0, high = arr.length() - 1;
    while (low < high) {
        int mid = (high + low)/2;
        if (arr[mid] == key) {
            return mid;
        }
        else if (arr[mid] < key) {
            low = mid + 1;
        }
        else {
            high = mid;
        }
    }
    if (arr[low] == key) {
        return low;
    }
    return -1;
}
```

### Template 3

```C
int binary (int[] arr; int key) {
    int low = 0, high = arr.length() - 1;
    while (low + 1 < high) {
        int mid = (low + high)/2;
        if (arr[mid] == key) {
            return mid;
        }
        else if (arr[mid] < key) {
            low = mid;
        }
        else {
            high = mid;
        }
    }
    if (nums[low] == key) {
        return low;
    }
    if (nums[high] == key) {
        return high;
    }
    return -1;
}
```
## RECURSIVE BINARY SEARCH  
Binary search can be implemented using recursion as well.  
### Implementation of recursive Binary search

```C
void binary(int arr[], int key, int low, int high) {
	int mid, flag = 0;
	if (low > high) {
		printf("Element not found");
	}
	mid = (low+high)/2;
	if (key < arr[mid]) {
		binary(arr, key, 0, mid - 1);
	}
	else if (key > arr[mid]) {
		binary(arr, key, mid+1, high);
	}
	else {
		flag = 1;
	}
	if (flag == 1) {
		printf("Element found at index %d", mid);
	}
}
```