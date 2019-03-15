## Bitmap：位图


一个byte占8个bit，如果用一个bit的 值来表示有或没有，也就是 二进制的0或1

那就可用 **bit的 0 或 1 代表某数组值有或没有**。0代表该数值没有出现过，1代表该数组值出现过

###### 2的10次方=10 的三次方

byte[index（N）] :索引号index是多少，位置号position(N)是哪一位

Index(N) 代表N的索引号，Position(N)代表N的位置号

### 公式： 

​	Index(N) = N/8 = N >> 3，代表那个字节
​	Position(N) = N%8 ,指在相应字节中的bit为



### 代码：

```java
public void add(int num)
{
   	 int index=num>>3 ;
    	int position=num%8 ;
   	 byte[index] |= 1<<position; //如图，将 1左移 position 位，使目标位置1
}
```

### 应用：

###### 1：大数据查重（分治）

###### 2：判断大数据中有没有该数（bit 位根据 0 或 1 来二分，需要分32次）







