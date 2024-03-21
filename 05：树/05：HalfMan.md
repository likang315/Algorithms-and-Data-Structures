### 哈夫曼树、最优树二叉树：

------

​	树的带权路径最小的二叉树

- 路径长度：路径上的分支数目
  - 从树中一个结点到另一个结点之间构成两个结点的路径
- 树的路径长度：从根到每一个结点的路径长度之和
- 带权路径长度：从该结点到树根之间的路径长度与结点上权的乘积
- 树的带权路径长度（WPL）：为树中所有叶子结点的带权路径长度之和

```java
// 链式创建HalfmanTree树，一般未知创建几个结点（装箱问题）
public class HalfNode {
    char word;
    int val;//权值
    HalfNode left=null;
    HalfNode right=null;

    HalfNode(char word,int val) {
        this.word=word;
        this.val = val;
    }
}
// 创建森林,存放叶子结点
public List creatForest(int n) {
    List<HalfNode> list=new LinkList<HalfNode>();
    for (int i = 0; i < n; i++) {
        char ch;
        int val;
        HalfNode node=new HalfNode();
        Scanner sc1=new Scanner(System.in);
        while(sc.hasNext())	{
            node.val=sc1.nextInt();
        }
        sc1.close();
        Scanner sc2=new Scanner(System.in);
        while(sc.hasNext()) {
            node.word=sc2.next().charAt(0);
        }
        sc2.close();
    }
    return list;
}
// 创建HalfMan树
public HalfNode  creatHalfTree(List<HalfNode> list) {
    // 只要nodes数组中还有2个以上的节点
    while (list.size() > 1) {
        //根据val排序
        Collections.sort(list);
        //获取权值最小的两个节点
        HalfNode left = list.get(nodes.size()-1);
        HalfNode right = list.get(nodes.size()-2);
        //父节点权值为两个子节点的权值之和
        HalfNode parent = new HalfNode('X', left.val + right.val);
        parent.left  = left;
        parent.right = right;

        //删除权值最小的两个节点
        list.remove(nodes.size()-1);
        list.remove(nodes.size()-2);

        //将新节点加入到集合中
        list.add(parent);
    }
	return list.get(0);
}
```

### 2：HalfmanCode

![HalfmanCode.png](https://github.com/likang315/Algorithms-and-Data-Structures/blob/master/8%EF%BC%9A%E4%BA%8C%E5%8F%89%E6%A0%91%EF%BC%8C%E6%A0%91%E5%92%8C%E6%A3%AE%E6%9E%97/HalfmanCode.png?raw=true)

```java
public class HalfNode{
    char word;
    int val;//权值
    HalfNode left=null;
    HalfNode right=null;
    HalfNode parent=null;
    //存储编码
    int[] code=new int[100]; 

    HalfNode(char word,int val) {
        this.word=word;
        this.val = val;
    }
}
//创建HalfmanCode,倒着存储，需倒着输出
void HalfmanCode(HalfNode list)
{
    int p, q, i;
    int[] code;
    for (i = 0; i < list.size(); i++)
    {
        code=list.get(i).code;
        code[0]=0;
        p = i;
        while (list.get(p).parent!= null)
        {
            q = list.get(p).parent;
            if (q.left == p)
                code[++code[0]] = 0;
            else
                code[++code[0]] = 1;
            p = q;
        }
    }
}
```





