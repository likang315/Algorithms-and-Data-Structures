### 跳表【SkipList】

------

[TOC]

##### 01：概述

- 对标的是平衡树(AVL Tree)，是一种 插入、删除、搜索 时间复杂度都是 **`O(logn)`** 的数据结构；
- 跳表玩的是有序链表【双向链表】

##### 02：结构

![SkpiList](/Users/likang/Code/Git/Algorithms-and-Data-Structures/02：链表/photo/SkpiList.png)

- 从索引层开始检索，比它大往后找，比它小往下找；
- level 的层数是随机生成的，不过有大小限制；