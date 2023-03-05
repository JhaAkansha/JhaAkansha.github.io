---
title: "Selection Sort"
date: 2022-08-06T14:20:15+05:30
#draft: true
---
***Runtime***: &nbsp; O(n²) &emsp; &emsp; &emsp; ***Memory*** : &nbsp; O(1)

* Find the smallest element using a linear scan and move it to the front.
* Using linear scane, traverse the array from the second element onwards and find the least element in this sub-array.
* Swap the second element with the least element in the sub-array (which will the second least element in the array). 
* Continue doing this until all elements are in place.  
The default implementation of selection sort is not stable.
## Implementation of SelectionSort 
```C
for (i = 0; i < n - 1; i++) {
	min_idx = i;
	for (j = i+1; j < n; j++) {
		if (arr[j] < arr[min_idx]){
			min_idx = j;
		}
	}
	if (min_idx != i) {
		swap(&arr[i], &arr[min_idx]);
	}
}
```