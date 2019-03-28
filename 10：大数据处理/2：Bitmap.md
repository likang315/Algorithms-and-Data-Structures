## Bitmap：位图


一个byte占8个bit，如果用一个bit的 值来表示有或没有，也就是 二进制的0或1

那就可用 **bit的 0 或 1 代表某数组值有或没有**。0代表该数值没有出现过，1代表该数组值出现过

###### 2的10次方=10 的三次方

byte[index（N）] :索引号index是多少，位置号position(N)是哪一位

Index(N) 代表N的索引号，Position(N)代表N的位置号

### 公式： 

​	Index(N) = N/8 = N >> 3，代表第几个字节
​	Position(N) = N%8 ,指在相应字节中的第几bit

![Bitmap.png](https://github.com/likang315/Algorithms-and-Data-Structures/blob/master/10%EF%BC%9A%E5%A4%A7%E6%95%B0%E6%8D%AE%E5%A4%84%E7%90%86/Bitmap.png?raw=true)

### 代码：

```java
public void add(int num)
{
   	 int index = num>>3 ;
     int position = num%8 ;
   	 byte[index] |= 1<<position; //如图，将 1左移 position 位，使目标位置1，和原来的位图做或操作
}
```

### 应用：

###### 1：快速排序，通过数字表示bit位

###### 2：查找，判断大数据中有没有该数（bit 位根据 0 或 1 来二分，需要分32次）

###### 3：大数据查重



### 例：URL 查重，hash（String url）

1：通过一种hash（String url）产生唯一的 value(int值)，应用 BItMap 算法

2：通过hash（String url） 产生可能出现重复value 值的，通过value % n 放入相应的块中，然后查重

​	因为相同的url产生相同的value ，最后分布到相同的块中，可以去重

3：通过多个 hashmap（）对象，序列化到磁盘上，需要一个最新的来记录哪一个是最新创建的hashmap（）队列



