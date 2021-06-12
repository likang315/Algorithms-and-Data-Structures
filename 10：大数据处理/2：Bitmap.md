### BitMap：位图

------


​	一个byte占8个bit，如果用一个bit的值来表示有或没有，也就是二进制的0或1，那就可用 **bit的 0 或 1 代表某数组值有或没有**，0 代表没有该数值，1代表有该数组值

- 2 ^10 = 1024 = 10 ^ 3
- byte[index(N)]：索引号index是多少，位置号 position(N) 是哪一位
- Index(N) 代表N的索引号，Position(N)代表N的位置号

##### 1：公式：

- 字节数 + bit 数
- Index(N) = N/8 = N >> 3，代表第几个字节
- Position(N) = N%8，指在相应字节中的第几bit

![BitMap](/Users/likang/Code/Git/Algorithms-and-Data-Structures/10：大数据处理/BigData/BitMap.png)

```java
// 将新数据添加到容器中
public void add(int num) {
    int index = num >> 3;
    int position = num % 8;
    // 将1左移 position 位，使目标位置1和原来的位图做或操作
    byte[index] |= 1 << position;
}
```

##### 2：应用：

1. 大数据快速排序，通过表示bit位来表示数字
2. 查找，判断大数据中有没有该数，和索引byte[index] 该值的bitmap表示想与，byte[index]值若不改改变，则含有
3. 大数据查重

###### 例：URL 查重，hash（String url）

1. 通过一种hash（String url）产生唯一的 value(int值)，然后应用 BItMap 算法
2. 通过hash（String url） 产生可能出现重复value 值的，通过value % n 放入相应的块中，然后查重
3. 通过多个 hashmap（）对象，序列化到磁盘上，需要一种结构来记录哪一个是最新创建的hashmap( )，队列

