---
title: "Big O"
date: 2022-07-14T12:09:29+05:30
#draft: true

---
![img](https://cdn-images-1.medium.com/max/1600/1*MojRMNBNOHLqwe5ak7hTug.png "Time - complexity Graph")
\
\
Big O is the used to decribe the efficiency of algorithms. Academics use big O, big θ and big Ω to decribe runtimes.
* Big O : Big O describes the upper bound on time. An algorithm that requires O(n) time can also be described as requiring O(n²), O(n³), O(2ⁿ) etc. The algorithm is at least as fast as any of these. Therefore, they are upper bounds on the runtime. For example, if x<13, it is also true that x<100, x<1000 etc.
* Big Ω :  Big Ω is a similar concept but for lower bound. If the runtime is decribed as Ω(n), then it can also be described by Ω(log n) and Ω(1). But, it won't be faster than those runtimes.
* Big θ : θ means both O and Ω. That is, an algorithm is θ(n) if its both O(n) and Ω(n). θ gives a tight bound on runtimes.
\
In industry, the meaning of O and θ seem to have been merged. Industry's meaning of big O is closer to what academics mean by θ. The runtime for an algorithm is described in three different ways:
* **Best Case :** If we consider the case of sorting an array, then if all the elements are equal, then quick sort will traverse the array only once giving the runtime O(n). This is the least runtime possible, hence it is the best case scenario. 
* **Worst Case :** If we get really unlucky and the pivot is repeatedly the biggest element in the array then we have the largest possible runtime. This is the worst case scenario.
* **Expected Case :** Usually, the conditions considered above don't happen. In general cases, the expected runtime for quick sort will be O(n log n).