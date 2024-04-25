### 二分查找（BinarySearch）

------

[TOC]

##### 01：二分查找

- 要求线性表必须是有序的
- ASL：O(logn)
- 二分递归**终止条件：(left > right)，继续条件：（left <= right）**
  - 原理：查看 mid 是不是检索的 key

```java
// 递归(半递归，没有回溯)
public int binarySearch(int[] a, int left, int right, int key) {
    int mid;
    if (left > right) {
        return -1;
    } else {
        mid = (left + right) / 2;
        if (a[mid] == key) {
            return mid;
        } else if (a[mid] > key) {
            return binarySearch(a, left , mid - 1, key);
        } else {
            return binarySearch(a, mid + 1, right, key);
        }
    }
}
// 非递归
public int binarySearch(int[] a, int left, int right, int key) {
    int mid;
    while (left <= right) {
        mid = (left + right) / 2;
        if (a[mid] == key) {
            return mid;
        } else if (a[mid] > key) {
            right = mid - 1;
        } else {
            left = mid + 1;
        }
    }
    return -1;
}
```

##### 02：排序数组中查找元素的第一个和最后一个位置

- ```java
  // 排序数组查找=二分
  static int[] solution(int[] nums, int target) {
      int left = 0;
      int right = nums.length - 1;
      while (left <= right) {
          int mid = (left + right) / 2;
          if (nums[mid] == target) {
              // 从二分后，边界处往里找
              while (nums[left] != target) {
                  left++;
              }
              while (nums[right] != target) {
                  right--;
              }
              return new int[] {left, right};
          } else if (nums[mid] < target) {
              left = mid + 1;
          } else {
              right = mid - 1;
          }
      }
      return new int[] {-1, -1};
  }
  ```

  