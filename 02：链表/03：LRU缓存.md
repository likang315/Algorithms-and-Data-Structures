### LRU

------

[TOC]

##### 01：概述

- LRU是**最近最少使用**页面置换算法(Least Recently Used)，也就是首先淘汰最长时间未被使用的页面；
- LFU是**最近最不常用**页面置换算法(Least Frequently Used)，也就是淘汰一定时期内被访问次数最少的页面；

##### 02：设计和实现一个  LRU (最近最少使用) 缓存机制

- 它应该支持以下操作： 获取数据 get 和 写入数据 put ；
- 获取数据 get(key) - 如果密钥 (key) 存在于缓存中，则获取密钥的值（总是正数），否则返回 -1。
- 写入数据 put(key, value) - 如果密钥不存在，则写入其数据值。当缓存容量达到上限时，它应该在**写入新数据之前删除最近最少使用的数据值**，从而为新的数据值留出空间。

##### 03：实现

1. 链表：效率低；
2. 哈希表 + 双向链表：LinkedHashMap
   - 在双向链表实现中，这里使用一个**伪头部和伪尾部标记界限**，这样在更新的时候就不需要检查是否是 null 节点；

```java
import java.util.Hashtable;
/**
 * 只需要在HashTable的基础上维护个双向链表即可
 */
public class LRUCache {
    // 定义双向链表节点
    class DLinkedNode {
        int key;
        int value;
        DLinkedNode prev;
        DLinkedNode next;
    }

    // Always add the new node right after head
    private void addNode(DLinkedNode node) {
        node.prev = head;
        node.next = head.next;
        head.next.prev = node;
        head.next = node;
    }

    // Remove an existing node from the linked list
    private void removeNode(DLinkedNode node){
        DLinkedNode prev = node.prev;
        DLinkedNode next = node.next;
        prev.next = next;
        next.prev = prev;
    }

    // Move certain node in between to the head.
    private void moveToHead(DLinkedNode node){
        removeNode(node);
        addNode(node);
    }

    // Pop the current tail
    private DLinkedNode popTail() {
        DLinkedNode res = tail.prev;
        removeNode(res);
        return res;
    }

    private Hashtable<Integer, DLinkedNode> cache = new Hashtable<Integer, DLinkedNode>();
    private int size;
    private int capacity;
    private DLinkedNode head, tail;

    public LRUCache(int capacity) {
        this.size = 0;
        this.capacity = capacity;

        head = new DLinkedNode();
        // head.prev = null;
        tail = new DLinkedNode();
        // tail.next = null;
        // 让两个互指
        head.next = tail;
        tail.prev = head;
    }

    public int get(int key) {
        DLinkedNode node = cache.get(key);
        if (node == null) 
            return -1;
        // move the accessed node to the head;
        moveToHead(node);
        return node.value;
    }

    public void put(int key, int value) {
        DLinkedNode node = cache.get(key);
        if (node == null) {
            DLinkedNode newNode = new DLinkedNode();
            newNode.key = key;
            newNode.value = value;

            cache.put(key, newNode);
            addNode(newNode);
            ++size;
            // 判断容量来实现LRU
            if (size > capacity) {
                // pop the tail
                DLinkedNode tail = popTail();
                cache.remove(tail.key);
                --size;
            }
        } else {
            // update the value.
            node.value = value;
            moveToHead(node);
        }
    }
}
```

