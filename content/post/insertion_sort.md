---
title: "Insertion Sort"
date: 2022-08-21T02:03:01+05:30
#draft: true
---
***Best Case Runtime***: &nbsp; O(n) &emsp; &emsp; &emsp; ***Worst Case Runtime*** : &nbsp; O(n²)

The method for implementation of insertion sort is similar to the one we use to arrange our cards. It is a stable sorting algorithm. The steps followed are:  
* Iterate from arr[1] to arr[n] over the array.
* Compare the current element (key) to its predecessor.
If the key element is smaller than its predecessor, compare it to the element before. Move the greater elements one position up to make space for the swapped element.
**Implementation of InsertionSort**  
```C
for (i = 0; i < n; i++) {
	key = arr[i];
	j = i - 1;
	while (j >= 0 && arr[j] > key) {
		arr[j+1] = arr[j];
		j-=1;
	}
	key = arr[j];
}
```

## Binary Insertion Sort
This method uses binary search to find the proper location to insert the selected item at each iteration. In normal insertion, sorting takes O(i) time (at ith iteration) in worst case, this can be reduced to O(log i). The algorithm, as a whole, still has the worst case running time of O(n²).  
### Implementation of binary Insertion Sort  
```C
int binarySearch(int a[], int item, int low, int high) {
	if (high <= low) {
		return ((item > a[low]) ? (low+1) : low)
	}
	int mid = (low+high)/2;
	if (item == a[mid]) {
		return mid+1;
	}
	if (item > a[mid]) {
		return (binarySearch(a, item, mid+1, high));
	}
}
void insertionSort(int a[], int n) {
	int i, loc, j, k, selected;
	for (i = 1; i < n; ++i) {
		j = i-1;
		selected = a[i];
		loc = binarySearch(a, selected, 0, j);
		while (j >= loc) {
			a[j+1] = a[j];
			j--;
		}
		a[j+1] = selected;
	}
}
```