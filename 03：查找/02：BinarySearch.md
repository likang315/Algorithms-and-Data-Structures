### 二分查找（BinarySearch）

------

[TOC]

##### 01：二分查找

- 要求线性表必须是有序的
- ASL：O(logn)
- 二分递归**终止条件：(left > right)，继续条件：（left <= right）**
  - 原理：查看mid是不是检索的key

```java
// 递归(半递归，没有回溯)
public int binarySearch(int[] a, int left, int right, int key) {
	int mid;
	if (left > right) {
    return -1;
  } else {
		mid = (left + right) / 2;
		if (a[mid] == key)
			return mid;
		else if (a[mid] > key)
				return binarysearch(a, left , mid - 1, key);
		else
				return binarysearch(a, mid + 1, right, key);
	}
}
// 非递归
public int binarySearch(int[] a, int left, int right, int key) {                                                         
	int mid;
	while (left <= right) {
		mid = (left + right) / 2;
		if (a[mid] == key)
			return mid;
		else if (a[mid] > key)
      right = mid - 1;
    else
			left = mid + 1;
	}
	return -1;
}
```