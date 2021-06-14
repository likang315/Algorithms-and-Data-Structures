### Top K 算法

------

##### 01：问题描述：有 N (N>1000000) 个数，求出其中的**前K个最小的数**（又被称作topK问题） 

- **解法一：**将N个数进行完全排序，从中选出排在前K的元素即为所求。目前来看快速排序，堆排序和归并排序都能达到，**通过Bitmap排序**，不能有重复值
  - 时间复杂度：O(nlogn)
- **解法二：**可以采用数据池的思想，选择其中前K个数作为数据池，后面的N-K个数与这K个数进行比较，若小于其中的任何一个数，则进行替换
  - 算法复杂度：O(n*k)
- **解法三：**把前 K 个数取出，构建一个小顶堆，遍历 n-k 个数，调整小顶堆，该算法不需要一次将原数组中的内容全部加载到内存中，而这正是海量数据处理必然会面临的一个关卡
  - 时间复杂度是O(nlogk)

##### 02：输出数组中的第K个最大元素【第k大】

- 全量堆排序，对大堆，截取至最后一个；
- 大数据时，取k个元素建最小堆，遍历数组，比堆顶元素大，则替换，堆调整，最后堆顶元素就是第k大；

```java
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

##### 03：N个倒序的数组整体最大的Top K

1. 合并两个有序数组，求出第一个数组的top K ，再用第一个topk的结果，求出和第二个数组的topK（），依次递归；
2. 借助最大堆，每次输出根结点，并判断根节点所在的数组有没有后继元素，若有，则替换，若无，则用最后一个叶子结点替换并且减小top；

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

// int[][] arr = {{917}, {558,224, 24}, {691,434,234, 56}};
static void print(int[][] a, int top) {
    HeapNode[] result = new HeapNode[a.length];
    for (int row = 0; row < result.length; row++) {
        result[row] = new HeapNode(a[row][0], row, 1);
    }
    buildHeap(result);

    int heapSize = result.length;
    for (int index = 0; index < top; index++) {
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

