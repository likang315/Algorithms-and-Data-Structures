### Top K 问题

------

[TOC]

##### 00：TopK 问题

###### 示例：有 N 个数（海量数据），求前K个最小的数

1. 将 N 个数进行**排序**，从中选出排在前K的元素即为所求。目前来看快速排序，堆排序和归并排序都能达到；
  - 时间复杂度：$O(nlogn)$
2. 可以采用**数据池**的思想，选择其中前K个数作为数据池，后面的 N-K 个数与这 K 个数进行比较，若小于其中的任何一个数，则进行替换；
  - 算法复杂度：$O(n*k)$
3. 取前 K 个数构键**小顶堆**，遍历 n-k 个数，调整小顶堆，该算法不需要一次将原数组中的内容全部加载到内存中，而这正是海量数据处理必然会面临的问题；
  - 时间复杂度是：$O(nlogk)$

##### 01：输出数组中的第 K 个最大元素（第 k 大）

- 少量数据时，全量堆排序，大顶堆（最大堆），截取至最后一个；
  - 时间复杂度：$O(nlogn)$

- 海量数据时，取 k 个元素建小顶堆，遍历数组，比堆顶元素大，替换堆顶，堆调整，最后堆顶元素就是第 k 大；

```java
// 建全量大顶堆，考堆排序
public int findKthLargest(int[] a, int k) {
    int heapSize = a.length;
    buildHeap(a, heapSize);
    for (int i = a.length - 1; i >= a.length - k + 1; i--) {
        swap(a, 0, i);
        --heapSize;
        heapify(a, 0, heapSize);
    }
    return a[0];
}
```

##### 02：N 个倒序的数组整体最大的 TopK

1. 合并两个有序数组，求出第一个数组的top K ，再用第一个topk的结果，求和第三个数组合并的 topK，依次递归；
2. 利用倒叙特性，用 N 个数组的头元素建最大堆，输出根结点，再判断根节点所在的数组是否有后继元素，若有，补上头结点，若无，则用最后叶子结点所在数组的头元素补上；

```java
static class HeapNode {
    // 当前值
    private int value;
    // 那个数组
    private int arrNum;
    // 当前数组下标
    private int index;
    public HeapNode(int value, int arrNum, int index) {
        this.value = value;
        this.arrNum = arrNum;
        this.index = index;
    }
}

// 建最大堆，时间复杂度O(nlogn)
static void buildHeap(HeapNode[] a) {
    // 从非叶子结点开始向上调整，叶子结点总比非叶子结点多1
    for (int i = a.length / 2 - 1; i >= 0; i--) {
        heapify(a, i, a.length);
    }
}

// 堆化，从a[i]向下调整堆
static void heapify(HeapNode[] a, int i, int heapSize) {
    int left = 2 * i + 1;
    int right = 2 * i + 2;
    int max = i;
    if (left < heapSize && a[left].value > a[max].value)
        max = left;
    if (right < heapSize && a[right].value > a[max].value)
        max = right;
    if (max != i) {
        swap(a, i, max);
        // 继续从当前结点向下进行堆调整，max为当前交换的结点
        heapify(a, max, heapSize);
    }
}

// 两数交换
static void swap(HeapNode[] a, int i, int j) {
    HeapNode temp = a[i];
    a[i] = a[j];
    a[j] = temp;
}

// int[][] arr = {{917}, {558, 224, 24}, {691, 434, 234, 56}};
static void print(int[][] a, int top) {
    HeapNode[] result = new HeapNode[a.length];
    for (int row = 0; row < result.length; row++) {
        result[row] = new HeapNode(a[row][0], row, 1);
    }
    buildHeap(result);

    int heapSize = result.length;
    for (int i = 0; i < top; i++) {
        HeapNode p = result[0];
        System.out.println(p.value);
        if (p.index < a[p.arrNum].length) {
            result[0].value = a[p.arrNum][p.index++];
        } else {
            swap(result, 0, --heapSize);
        }
        heapify(result, 0, heapSize);
    }
}
```

