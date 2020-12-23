---
title: Data Structure and Algorithm
tags:
    - cs
date: 2018-11-25 22:54:25
categories: computer science
---


动图演示算法: https://visualgo.net/zh
数据结构: https://www.cnblogs.com/skywang12345/p/3603935.html
python 实现数据结构: https://github.com/xiaoyuside/algorithm-python
java 实现数据结构算法: https://github.com/buptdavid/datastructure
Suffix Trees - https://github.com/npgall/concurrent-trees
Radix Trees

[面试](https://blog.csdn.net/u012414189/article/details/83832161?utm_medium=distribute.pc_relevant.none-task-blog-BlogCommendFromMachineLearnPai2-2.nonecase&depth_1-utm_source=distribute.pc_relevant.none-task-blog-BlogCommendFromMachineLearnPai2-2.nonecase)

https://github.com/kdn251/interviews/blob/master/README-zh-cn.md


https://github.com/labuladong/fucking-algorithm
https://github.com/liweiwei1419/LeetCode-Solutions-in-Good-Style
https://github.com/greyireland/algorithm-pattern
https://github.com/geekxh/hello-algorithm

https://github.com/apachecn/apachecn-algo-zh

https://github.com/facert/python-data-structure-cn
https://github.com/keon/algorithms

https://github.com/xtaci/algorithms cpp 中文
<!--more-->

<!-- TOC -->

- [数据结构总结](#数据结构总结)
  - [存储方式](#存储方式)
  - [基本操作](#基本操作)
  - [时间复杂度](#时间复杂度)
    - [计算方法](#计算方法)
    - [常数复杂度](#常数复杂度)
    - [线性复杂度](#线性复杂度)
    - [对数复杂度](#对数复杂度)
    - [平方复杂度](#平方复杂度)
    - [指数复杂度](#指数复杂度)
- [数组](#数组)
- [栈](#栈)
- [队列](#队列)
- [链表](#链表)
- [树](#树)
  - [树的基本概念and一般结构](#树的基本概念and一般结构)
  - [二叉查找树](#二叉查找树)
    - [非平衡二叉查找树](#非平衡二叉查找树)
      - [BST(Binary Search Tree)-查找树的一般结构](#bstbinary-search-tree-查找树的一般结构)
      - [Splay Tree 伸展树](#splay-tree-伸展树)
    - [平衡二叉树](#平衡二叉树)
      - [AVL树](#avl树)
      - [红黑树](#红黑树)
  - [多路平衡查找树](#多路平衡查找树)
    - [2-3树(二三树)](#2-3树二三树)
    - [B树(B-tree)](#b树b-tree)
      - [b树的概念](#b树的概念)
      - [b树的特征](#b树的特征)
    - [B+tree(B加树)](#btreeb加树)
      - [b+tree定义](#btree定义)
      - [比b-tree好的点](#比b-tree好的点)
    - [B*树](#b树)
    - [前缀树(Trie树/字典树)](#前缀树trie树字典树)
  - [哈夫曼树(Huffman Tree)](#哈夫曼树huffman-tree)
- [堆](#堆)
  - [二叉堆](#二叉堆)
  - [堆排序](#堆排序)
- [图](#图)
  - [图的概念](#图的概念)
- [哈希表(hash-table)](#哈希表hash-table)
- [排序算法](#排序算法)
  - [选择排序(select Sort)](#选择排序select-sort)
  - [归并排序(merge sort)](#归并排序merge-sort)
  - [冒泡排序(bubble sort)](#冒泡排序bubble-sort)
- [查找算法](#查找算法)
  - [顺序查找](#顺序查找)
  - [有序表查找](#有序表查找)
  - [线性索引查找](#线性索引查找)
- [索引](#索引)
  - [线性索引](#线性索引)
    - [稠密索引](#稠密索引)
    - [分块索引](#分块索引)
    - [倒排索引](#倒排索引)
  - [树形索引](#树形索引)
  - [多级索引](#多级索引)

<!-- /TOC -->


# 数据结构总结

## 存储方式

数据结构的存储方式只有两种：数组（顺序存储）和链表（链式存储）

- 数组由于是紧凑连续存储,可以随机访问，通过索引快速找到对应元素，而且相对节约存储空间。但正因为连续存储，内存空间必须一次性分配够，所以说数组如果要扩容，需要重新分配一块更大的空间，再把数据全部复制过去，时间复杂度 O(N)；而且你如果想在数组中间进行插入和删除，每次必须搬移后面的所有数据以保持连续，时间复杂度 O(N)。

- 链表因为元素不连续，而是靠指针指向下一个元素的位置，所以不存在数组的扩容问题；如果知道某一元素的前驱和后驱，操作指针即可删除该元素或者插入新元素，时间复杂度 O(1)。但是正因为存储空间不连续，你无法根据一个索引算出对应元素的地址，所以不能随机访问；而且由于每个元素必须存储指向前后元素位置的指针，会消耗相对更多的储存空间。

比如说「队列」、「栈」这两种数据结构既可以使用链表也可以使用数组实现。用数组实现，就要处理扩容缩容的问题；用链表实现，没有这个问题，但需要更多的内存空间存储节点指针

「图」的两种表示方法，邻接表就是链表，邻接矩阵就是二维数组。邻接矩阵判断连通性迅速，并可以进行矩阵运算解决一些问题，但是如果图比较稀疏的话很耗费空间。邻接表比较节省空间，但是很多操作的效率上肯定比不过邻接矩阵。

「散列表」就是通过散列函数把键映射到一个大数组里。而且对于解决散列冲突的方法，拉链法需要链表特性，操作简单，但需要额外的空间存储指针；线性探查法就需要数组特性，以便连续寻址，不需要指针的存储空间，但操作稍微复杂些。

「树」，用数组实现就是「堆」，因为「堆」是一个完全二叉树，用数组存储不需要节点指针，操作也比较简单；用链表实现就是很常见的那种「树」，因为不一定是完全二叉树，所以不适合用数组存储。为此，在这种链表「树」结构之上，又衍生出各种巧妙的设计，比如二叉搜索树、AVL 树、红黑树、区间树、B 树等等

## 基本操作

最主要的操作就是 遍历

- 线性遍历, 比如用 for 循环遍历一个数组; 比如链表, 既可以用 for 循环线性遍历, 也能用 递归遍历

- 非线性(递归) 遍历, 比如 二叉树递归遍历



## 时间复杂度

### 计算方法

- 去掉常数项。
- 只保留最高阶项。
- 去掉最高结项的系数


### 常数复杂度

O(1) - 一般是顺序执行的代码

```cpp
/**
 * 高斯算法, 常数复杂度
 */
void gaosi() {
    const int begin = 1;
    const int end = 4;

    int sum = (begin + end) * end / 2;
    cout << "arr sum = " << sum << endl;
}

```

### 线性复杂度 

O(n) - 一层循环

```cpp
/**
 * 线性复杂度
 */
void changshu() {
    for (int i = 0; i <; ++i) {
        printf("xxx");
    }
}
```


### 对数复杂度 

O(logn) 

```cpp
/**
 * 对数复杂度
 *
 * 假设会执行 x 次, 则 2^x = n, x = log n
 *
 */
void duishu() {
    const int n = 10;
    for (int i = 1; i < n; i = i * 2) {
        printf("xxx");
    }
}

```

### 平方复杂度 

O(n^2)

```cpp
/**
 * 平方复杂度
 *
 * 当 i = 0, 内循环执行 n 次
 * 当 i = 1, 内循环执行 n - 1 次
 * ...
 *
 * sum = n + (n-1) + (n-2) + ... + 1 = (n + 1) * n /2 约等于 n^2
 *
 */
void pingfang() {
    int n;
    for (int i = 0; i < n; ++i) {
        // 注意这里 j = i, 不是 从 0 开始的, 但是最终计算出来复杂度还是 n^2, 
        for (int j = i; j < n; ++j) {
            printf("xxx");
        }
    }
}

```


### 指数复杂度 

O(2^n)

# 数组

数组有上界和下界，数组的元素在上下界内是连续的, 因此 随机访问速度快; 一旦定义, 容量就固定了, 再增删会涉及到数组的复制, 那将是耗时的操作

多维数组(数组的元素为数组)

动态数组: Java 提供了 ArrayList和Vector

- Insert - 在某个索引处插入元素
- Get - 读取某个索引处的元素
- Delete - 删除某个索引处的元素
- Size - 获取数组的长度

# 栈

栈中的元素采用LIFO (Last In First Out)，即后进先出。可以想象成一个桶 bucket

典型应用: 实现 ctrl + z 撤销, 把之前的应用状态(限制个数)保存到内存中，最近的状态放到第一个

```java
- Push — 推入, 在栈的最上方插入元素
- Pop — 弹出, 返回栈最上方的元素，并将其删除
- isEmpty — 查询栈是否为空
- Top/peek — 返回栈最上方的元素，并不删除
/**
 * ArrayStack - 模拟 Stack 类, 数组实现
 */
public class ArrayStack<T> {

    private T[] bucket;
    private int count; // 元素个数

    public ArrayStack(Class<T> clazz, int size) {
        Object newInstance = Array.newInstance(clazz, size);
        bucket = (T[]) newInstance;
        count = 0; // 初始化为 0 个 元素
    }

    public void push(T value) {
        bucket[count] = value;
        count = count + 1;
    }

    public T pop() {
        int index = count -1;
        T ret = bucket[index];
        bucket[index] = null;
        count = count -1;
        return ret;
    }

    public T top() {
        int index = count -1;
        return bucket[index];
    }
}

```

# 队列

队列(Queue)与栈类似，都是采用线性结构存储数据。它们的区别在于，栈采用LIFO方式，而队列采用先进先出，即FIFO(First in First Out), 

队列只允许在"队首"进行删除操作，而在"队尾"进行插入操作。可以想象成一条队伍


```java
Enqueue — 在队列末尾插入元素
Dequeue — 将队列第一个元素删除
isEmpty — 查询队列是否为空
Top — 返回队列的第一个元素
/**
 * 数组实现的队列
 *
 * @author xiaoyu
 * @since 1.0
 */
public class ArrayQueue<T> {

    private T[] data;
    private int count;

    public ArrayQueue(Class<T> clazz, int size) {
        Object newInstance = Array.newInstance(clazz, size);
        data = (T[]) newInstance;
        count = 0;
    }

    public T deQueue() {
        T ret = data[0];
        data[0] = null;
        for (int i = 0; i < count; i++) {// 顺次向前挪动
            data[i] = data[i+1];
        }
        count = count -1;
        return ret;
    }

    public void enQueue(T value) {
        data[count] = value;
        count = count + 1;
    }

    public T top() {
        return data[0];
    }

    public boolean empty() {
        if (count == 0) {
            return true;
        }
        if (count < 0) {
            throw new RuntimeException("count < 0");
        }
        return false;
    }
}

```

# 链表

链表是一系列节点组成的链，每一个节点保存了数据以及指向下一个节点的指针。链表头指针指向第一个节点，如果链表为空，则头指针为空或者为null。

场景: 实现文件系统, 哈希表

- 单向链表  - 节点的链接方向是单向的；相对于数组来说，单链表的的随机访问速度较慢，但是单链表删除/添加数据的效率很高。
- 双向链表 - 一般是双向循环链表, 每个数据结点中都有两个指针，分别指向直接后继和直接前驱



双向循环链表图示:

![alt](Snipaste_2018-11-25_22-52-23.png)

```java
InsertAtEnd — 在链表结尾插入元素
InsertAtHead — 在链表开头插入元素
Delete — 删除链表的指定元素
DeleteAtHead — 删除链表第一个元素
Search — 在链表中查询指定元素
isEmpty — 查询链表是否为空
/**
 * DoubleLink 双向循环链表 - 模拟 单列的 LinkedHashMap
 */
public class DoubleLink<T> {

    private int size;
    private Node<T> head;// important
                // 作为基本坐标

    private class Node<T> {
    
        private Node<T> prev;
        private Node<T> next;
        private T value;

        public Node(T value, Node<T> prev, Node<T> next) {
            this.next = next;
            this.prev = prev;
            this.value = value;
        }
    }

    public DoubleLink() {
        head = new Node<>(null, null, null);
        head = head.next = head.prev;
        this.size = 0;
    }

    /**
     * insert element to the end
     */
    public void insertAtEnd(T value) {
        Node<T> prev = head.prev;// 前一个节点
        Node<T> next = head; // 后一个节点
        Node<T> node = new Node<T>(value, prev, next);
        prev.next = node;
        next.prev = node;

        size++;
    }

    public void insertAtHead(T value) {
        Node<T> prev = head;
        Node<T> next = head.next;
        Node<T> node = new Node<T>(value, prev, next);
        prev.next = node;
        next.prev = node;

        size++;
    }

    public void insertAt(int index, T value) {
        DoubleLink<T>.Node<T> findNode = this.findNode(index);
        Node<T> prev = findNode.prev;
        Node<T> next = findNode.next;
        DoubleLink<T>.Node<T> node = new Node<T>(value, prev, next);
        prev.next = node;
        next.prev = node;

        size++;
    }

    public void deleteAt(int index) {
        DoubleLink<T>.Node<T> findNode = this.findNode(index);
        Node<T> prev = findNode.prev;
        Node<T> next = findNode.next;
        prev.next = next;
        next.prev = prev;

        findNode = null;
        size--;
    }

    /**
     * find specific node
     * @param index index, start with 0
     * @return
     */
    public Node<T> findNode(int index) {
        Node<T> ret = null;

        if (index < 0) {
            throw new RuntimeException("index can not less than 0");
        }

        // 参考 LinkedHashmap 实现
        if (index <= size / 2) {// 正向
            Node<T> first =  head.next;
            for (int i = 0; i < index; i++) {
                ret = first.next;
            }
            return ret;
        }

        // 反向查找
        Node<T> last = head.prev;
        for (int i = size - 1; i >= index; i--) {
            ret = last.prev;
        }
        return ret;
    }

    public boolean empty() {
        if (size == 0) {
            return true;
        }
        return false;
    }
}

```


# 树

## 树的基本概念and一般结构

树(Tree)是一个分层的数据结构，由节点和连接节点的边组成。树是一种特殊的图，它与图最大的区别是没有循环。

核心思想是使用 "二分法", "数据平衡策略" 来提高查询效率

几个概念:

- root - 根节点
- parent - 这个节点具有子节点
- child - 这个节点具有父节点
- leaf - 这个节点没有子节点
- sibling - 同等级(同辈)节点
- 一颗树深度为h，最大层数为k，深度与最大层数相同，k=h, 叶子数为2h; 第k层的结点数最多是：2^(k-1); 总结点数是：2^(k-1)，且总节点数一定是奇数
- 树的高度为最大层次。即空的二叉树的高度是0，非空树的高度等于它的最大层次(根的层次为1，根的子节点为第2层，依次类推)。

N叉树(N-ary Tree)
平衡树(Balanced Tree)
二叉树(Binary Tree) - 常见
二叉查找树(Binary Search Tree) - 常见
平衡二叉树(AVL Tree)
红黑树(Red Black Tree)
2-3树(2–3 Tree)



二叉树一般结构：二叉树的每个结点至多只有二棵子树，二叉树的子树有左右之分。二叉树的第i层至多有2^(i-1)个结点；

![alt](Snipaste_2018-08-26_16-44-57.png)


- 满二叉树：除叶子结点外的所有结点均有两个子结点。节点数达到最大值，所有叶子结点必须在同一层上

- 完全二叉树：若设二叉树的深度为h，除第 h 层外，其它各层 (1～(h-1)层) 的结点数都达到最大个数，第h层所有的结点都连续集中在最左边，这就是完全二叉树。

图示:

![alt](Snipaste_2018-11-26_17-50-50.png)

## 二叉查找树

### 非平衡二叉查找树

#### BST(Binary Search Tree)-查找树的一般结构

- 定义: 它是特殊的二叉树, 节点可比较：-- `任意节点, left subNode < node < right subNode`

    对于二叉树，假设x为二叉树中的任意一个结点，x节点包含关键字key，节点x的key值记为key[x]。如果y是x的左子树中的一个结点，则key[y] <= key[x]；如果y是x的右子树的一个结点，则key[y] >= key[x]。那么，这棵树就是二叉查找树

- 前驱和后继

    - 节点的前驱：是该节点的左子树中的最大节点。即，"二叉树中数据值小于该结点"的"最大结点"

    - 节点的后继：是该节点的右子树中的最小节点。即，"二叉树中数据值大于该结点"的"最小结点"

- 特征:

    ```java
    (01) 若任意节点的左子树不空，则左子树上所有结点的值均小于它的根结点的值；
    (02) 任意节点的右子树不空，则右子树上所有结点的值均大于它的根结点的值；
    (03) 任意节点的左、右子树也分别为二叉查找树。
    (04) 没有键值相等的节点（no duplicate nodes）。

    ```

- 时间复杂度  - 平衡状态下, B树的搜索性能逼近二分查找(插入和查找的时间复杂度均为`O(log n)`), 但是在最坏的情况下仍然会有`O(n)`的时间复杂度, n 可以看作 高度

    不平衡是怎么造成的? 
    
    比如 插入的序列是有序的时; 可以通过随机化建立二叉搜索树来尽量的避免这种情况

    再比如 由于在删除时，我们总是选择将待删除节点的后继代替它本身，这样就会造成总是右边的节点数目减少，以至于树向左偏沉

- 插入过程: (https://lufficc.com/blog/binary-search-tree https://www.cnblogs.com/MrListening/p/5782752.html)

  - 若当前的二叉查找树为空，则插入的元素为根节点

  - 若插入的元素值小于根节点值，则将元素插入到左子树中

  - 若插入的元素值不小于根节点值，则将元素插入到右子树中

- 删除过程(希望删除节点 p)

  - p为叶子节点，直接删除该节点，再修改其父节点的指针（注意分是根节点和不是根节点）

  - p为单支节点（即只有左子树或右子树）。让p的子树与p的父亲节点相连，删除p即可

  - p的左子树和右子树均不空 - 找到 后继/前驱节点 y 移动过来代替 p, 然后连接 y parent 和 y 的 child (对于 后继: 让 y 的 parent 成为 y 的右子树 的 parent; 对于 前驱: 让 y 的 parent 成为 y 的左子树的 parent )

    为什么要找 后继/前驱? - 因为 他们一定是单支节点(后继没有左子树/前驱没有右子树), 这种节点容易移动, 并且满足节点的大小关系

- 搜索过程: 从根结点开始，如果查询的关键字与结点的关键字相等，那么就命中; 否则，如果查询关键字比结点关键字小，就进入左子树；如果比结点关键字大，就进入右子树；如果左子树或右子树的指针为空，则报告找不到相应的关键字；

  如果B树的所有非叶子结点的左右子树的结点数目均保持差不多（平衡），那么B树的搜索性能逼近二分查找(插入和查找的时间复杂度均为O(log n))；它比连续内存空间的二分查找的优点是，改变B树结构（插入与删除结点）不需要移动大段的内存数据 (比如 b tree在插入节点时候, 只需要将指针指向新增节点, 原有数据在内存中的位置不变)

  相反, 如果左右子树不平衡, 这时候的搜索性能已经是线性的了 (此时是最坏的情况, 会有O(n)的时间复杂度), 要避免这样的不平衡 b tree

  实际使用的B树都是在原B树的基础上加上平衡算法，即“`平衡二叉树`”（binary balanced tree，又称AVL树）；如何保持B树结点分布均匀的平衡算法是平衡二叉树的关键；平衡算法是一种在B树中插入和删除结点的策略；



- 几种遍历方式 - 前序遍历、中序遍历、后序遍历3种方式。 - 对二叉查找树进行中序遍历，即可得到有序(从小到大)的数列

ref: https://blog.csdn.net/qq_33243189/article/details/80222629

```java
2.1 前序遍历 - parent节点排最先，然后同级先左后右
若二叉树非空，则执行以下操作：
(01) 访问根结点；
(02) 先序遍历左子树；
(03) 先序遍历右子树。

private void preOrder(BSTNode<T> tree) {
    if(tree != null) {
        System.out.print(tree.key+" ");
        preOrder(tree.left);
        preOrder(tree.right);
    }
}

public void preOrder() {
    preOrder(mRoot);
}
 
////////////////////////////////////////

2.2 中序遍历 -  先左后parent最后右

若二叉树非空，则执行以下操作：
(01) 中序遍历左子树；
(02) 访问根结点；
(03) 中序遍历右子树。

private void inOrder(BSTNode<T> tree) {
    if(tree != null) {
        inOrder(tree.left);
        System.out.print(tree.key+" ");
        inOrder(tree.right);
    }
}

public void inOrder() {
    inOrder(mRoot);
}
 ////////////////////////////////////////////////////

2.3 后序遍历 - 先左后右最后parent

若二叉树非空，则执行以下操作：
(01) 后序遍历左子树；
(02) 后序遍历右子树；
(03) 访问根结点。

private void postOrder(BSTNode<T> tree) {
    if(tree != null)
    {
        postOrder(tree.left);
        postOrder(tree.right);
        System.out.print(tree.key+" ");
    }
}

public void postOrder() {
    postOrder(mRoot);
}

```

遍历结果图示:

![alt](Snipaste_2018-11-26_18-21-36.png)

Java实现

```java
遍历: 前序, 中序, 后序遍历
find 查找
max 最大值
min 最小值
predecessor 前驱节点
successor 后继节点
insert 插入
remove 删除
destroy 销毁二叉树

/**
 * @author xiaoyu
 * @since 1.0
 */
public class BinarySearchTree<T extends Comparable<T>> {

    private Node<T> root;// root node

    public class Node<T extends Comparable<T>> {
        private T       value;
        private Node<T> parent;
        private Node<T> left;
        private Node<T> right;

        public Node(T value, Node<T> parent, Node<T> left, Node<T> right) {
            this.value = value;
            this.parent = parent;
            this.left = left;
            this.right = right;
        }
    }

    public BinarySearchTree(T value) {
        root = new Node<>(value, null, null, null);
    }

    // ------------------------------------------------

    /**
     * 前序遍历 - parent节点排最先，然后同级先左后右
     *
     * @param node
     */
    public void preOrderTraverse(Node<T> node) {
        if (node == null) {
            throw new RuntimeException("can not traverse null node");
        }
        System.out.print(node.value + " ");// print current node's value
        preOrderTraverse(node.left);   //
        preOrderTraverse(node.right);
    }

    /**
     * 中序遍历 - 先左后parent最后右
     *
     * @param node
     */
    public void middleOrderTraverse(Node<T> node) {
        if (node == null) {
            throw new RuntimeException("cannot traverse null node");
        }
        middleOrderTraverse(node.left);
        System.out.print(node.value + " ");
        middleOrderTraverse(node.right);
    }

    /**
     * 后序遍历 - 先左后右最后parent
     *
     * @param node
     */
    public void postOrderTraverse(Node<T> node) {
        if (node == null) {
            throw new RuntimeException("cannot traverse null node");
        }
        postOrderTraverse(node.left);
        postOrderTraverse(node.right);
        System.out.println(node.value + " ");
    }

    // ------------------------------------------------------

    /**
     * 使用递归查找
     *
     * @param enterNode
     * @param value
     * @return
     */
    public Node<T> find(Node<T> enterNode, T value) {
        if (enterNode == null) {
            throw new RuntimeException("cannot find with null enterNode");
        }

        int tmp = value.compareTo(enterNode.value);
        if (tmp == 0) {
            return enterNode;
        } else if (tmp < 0) {  // value < enterNode.value
            return find(enterNode.left, value);
        } else {
            return find(enterNode.right, value);
        }
    }

    /**
     * 没有递归的查找
     *
     * @param enterNode
     * @param value
     * @return
     */
    public Node<T> find_2(Node<T> enterNode, T value) {
        if (enterNode == null) {
            throw new RuntimeException("cannot find with null enterNode");
        }
        while (enterNode != null) {// 如果 enterNode == null 了， 证明循环
            // 到叶子节点了， 跳出, 返回null
            int tmp = value.compareTo(enterNode.value);
            if (tmp == 0) {
                return enterNode;
            } else if (tmp < 0) {
                enterNode = enterNode.left;
            } else {
                enterNode = enterNode.right;
            }
        }
        return null;
    }

    public Node<T> find(T value) {
        return this.find(root, value);
    }

    // -------------------------------------------------------------

    /**
     * 最大值
     *
     * @param enterNode
     * @return
     */
    public Node<T> maxNode(Node<T> enterNode) {
        while (true) {
            Node<T> right = enterNode.right;
            if (right == null) { // 证明此时enterNode 是叶子节点了
                return enterNode;
            }
            enterNode = right;
        }
    }

    /**
     * 最小值
     *
     * @param enterNode
     * @return
     */
    public Node<T> minNode(Node<T> enterNode) {
        while (true) {
            Node<T> left = enterNode.left;
            if (left == null) {
                return enterNode;
            }
            enterNode = left;
        }
    }

    // ------------------------------------

    /**
     * 节点的前驱：是该节点的左子树中的最大节点。即比当前节点小的最大节点
     *
     * @param enterNode
     * @return
     */
    public Node<T> predecessor(Node<T> enterNode) {
        Node<T> left = enterNode.left;
        // enterNode 存在 left node
        //        p
        //      /  \
        //     *    *
        //      \
        //      ...
        //       \
        //        o
        //    - 那么只需要 以这个left node 为 根节点， 找到最大值即可, 即这里的 o
        if (left != null) {
            return this.maxNode(left);
        }

        // 如果 enterNode 没有 left node，分两种情况
        // 1. enterNode 自身是一个 right node
        //        o
        //      /  \
        //     *    p
        //          \
        //     - 找到 parent, 即这里的 o， 返回parent (这个 parent 就是比 enterNode 小的最大节点)
        // 2. enterNode 自身是一个left node
        //      o
        //       \
        //        *
        //      /  \
        //     p    *
        //     \
        //  - 则查找"enterNode的最低的父结点，并且该父结点要具有右孩子", 即这里的 o，找
        //        到的这个"最低的父结点"就是"x的前驱结点"
        Node<T> parent = enterNode.parent;
        if (parent == null) {// enterNode is root
            return null;
        }
        if (enterNode == parent.right) {
            return parent;
        }
        if (enterNode == parent.left) {
            // 循环找 parent
            while (true) {
                enterNode = parent; // enterNode 指针移动到 parent
                parent = enterNode.parent; // parent 指针移动到 新的 parent
                if (enterNode == parent.right) {
                    return parent;
                }
            }
        }
        throw new RuntimeException(MessageFormat.format(
                "子节点{0} 和 父节点 {1} 断开了", enterNode, parent));
    }

    /**
     * 节点的后继：是该节点的右子树中的最小节点， 即比当前节点大的最小节点
     *
     * @param enterNode
     * @return
     */
    public Node<T> successor(Node<T> enterNode) {
        // 如果存在 right node，则返回 以这个 right node 为子树的最小节点
        Node<T> right = enterNode.right;
        if (right != null) {
            return minNode(right);
        }

        Node<T> parent = enterNode.parent;
        if (parent == null) {
            return null;
        }
        if (enterNode == parent.left) {
            return parent;
        }
        if (enterNode == parent.right) {
            while (true) {
                enterNode = parent;
                parent = enterNode.parent;
                if (enterNode == parent.left) {
                    return parent;
                }
            }
        }
        throw new RuntimeException(MessageFormat.format(
                "子节点{0} 和 父节点 {1} 断开了", enterNode, parent));
    }

    // ----------------------------------------------

    /**
     * insert node into the tree
     *
     * @param node 待插入节点
     */
    public void insert(Node<T> node) {
        Node<T> enterPoint = this.root;
        while (true) {
            Node<T> enterPointTmp = enterPoint;// 暂存指针移动前的 enterPoint

            int tmp = node.value.compareTo(enterPoint.value);
            if (tmp == 0) {
                throw new RuntimeException("value already exist");
            } else if (tmp < 0) {
                enterPoint = enterPoint.left;
            } else {
                enterPoint = enterPoint.right;
            }

            if (enterPoint == null) {
                enterPoint = enterPointTmp;
                break;
            }
        }

        int tmp = node.value.compareTo(enterPoint.value);
        if (tmp == 0) {
            throw new RuntimeException("value already exist");
        } else if (tmp < 0) {
            enterPoint.left = node;
        } else {
            enterPoint.right = node;
        }

    }

    // ------------------------------------------

    /**
     * delete specific node
     * @param node
     */
    public void delete(Node<T> node) {
        // 若 node 为 leaf节点，直接删除
        //       *           *
        //      /             \
        //     p               p
        // 若 node 为 "只有一个子节点", 这个子节点为 singleSon, 那么用 singleSon 代替 node 即可
        //       p       p
        //      /         \
        //     *           *
        // 若 node 为 "有两个子节点",   需要找node的后继节点successor 或者 前驱 predecessor, 替换 node, 分 2 种情况
        //      1. successor 为 node 的直接子节点(即右节点)
        //         p
        //        / \
        //       *   su
        //          - 直接用 successor 替代 node
        //     2. successor 不为 node 的直接子节点(即不是右节点)
        //         p
        //        / \
        //       *   *
        //          /
        //         su
        //          - 用 successor 替换 node
        // # 为什么要找 successor代替node? - 想要用 o 代替 node , o 需要 满足 node.left < o (node) < node.right
        //      而 successor, predecessor的位置满足 node.left < predecessor < o (node) < successor < node.right
        Node<T> left = node.left;
        Node<T> right = node.right;
        Node<T> parent = node.parent;

        if (left ==null && right == null) {// 没有子节点
            if (parent.left == node) {
                parent.left = null;
            }
            if (parent.right == null) {
                parent.right = null;
            }
        }

        if (left != null && right != null) {// 有两个子节点
            Node<T> successor = this.successor(node);
            if (parent.left == node) {
                parent.left = successor;
            }
            if (parent.right == node) {
                parent.right = successor;
            }
            successor.parent = parent;

            left.parent = successor;
            successor.left = left;

            if (right != successor) {
                right.parent = successor;
                successor.right = right;
            }

        }

        // 只有一个子节点
        Node<T> son = null;
        if (left != null) {
            son = left;
        }
        if (right != null) {
            son = right;
        }
        if (parent.left == node) {
            parent.left = son;
        }
        if (parent.right == node) {
            parent.right = son;
        }

        node.parent = node.left = node.right = null;// 删除node
    }

    /**
     * destroy tree
     */
    public void destroy(Node<T> enterNode) {
        if (enterNode == null) {
            return;
        }
        Node<T> left = enterNode.left;
        if (left != null) {
            destroy(left);
        }
        Node<T> right = enterNode.right;
        if (right != null) {
            destroy(right);
        }
        enterNode = null;
    }

    public void destroy() {
        destroy(root);
    }

}


```

#### Splay Tree 伸展树

伸展树(Splay Tree)是特殊的二叉查找树。它的特殊是指，它除了`本身是棵二叉查找树`之外，它还具备一个特点: `当某个节点被访问时，伸展树会通过旋转使该节点成为树根`。这样做的好处是，下次要访问该节点时，能够迅速的访问到该节点。

### 平衡二叉树

#### AVL树

http://www.cnblogs.com/skywang12345/p/3577479.html

相比于"二叉查找树"，它的特点是：AVL树中任何节点的`左右两个子树的高度最大差别为1`, 所以它也被称为高度平衡树; 没有值相等重复的节点

这个方案很好的解决了二叉查找树退化成链表的问题，把插入，查找，删除的时间复杂度最好情况和最坏情况都维持在`O(logN)`

不平衡分四种情况

- LL - LeftLeft，也称为"左左"。插入或删除一个节点后，根节点的左子树的左子树还有非空子节点，导致"根的左子树的高度"比"根的右子树的高度"大2，导致AVL树失去了平衡。
- LR：LeftRight，也称为"左右"。插入或删除一个节点后，根节点的左子树的右子树还有非空子节点，导致"根的左子树的高度"比"根的右子树的高度"大2，导致AVL树失去了平衡
- RL：RightLeft，称为"右左"。插入或删除一个节点后，根节点的右子树的左子树还有非空子节点，导致"根的右子树的高度"比"根的左子树的高度"大2，导致AVL树失去了平衡。
- RR：RightRight，称为"右右"。插入或删除一个节点后，根节点的右子树的右子树还有非空子节点，导致"根的右子树的高度"比"根的左子树的高度"大2，导致AVL树失去了平衡

图示:

![alt](Snipaste_2018-11-29_21-55-02.png)![alt](Snipaste_2018-11-29_21-55-29.png)

对应的旋转操作

LL失去平衡的情况，可以通过一次旋转让AVL树恢复平衡, LL旋转是围绕"失去平衡的AVL根节点"进行的 - 将旋转节点变为右子节点

![alt](Snipaste_2018-11-29_22-04-29.png)

旋转代码：

```java
private AVLTreeNode<T> leftLeftRotation(AVLTreeNode<T> k2) {
    AVLTreeNode<T> k1;

    k1 = k2.left;
    k2.left = k1.right;
    k1.right = k2;

    k2.height = max( height(k2.left), height(k2.right)) + 1;
    k1.height = max( height(k1.left), k2.height) + 1;

    return k1;
}

```

RR是与LL对称的情况 - 将旋转节点变为左子节点

![alt](Snipaste_2018-11-30_12-46-04.png)

```java
private AVLTreeNode<T> rightRightRotation(AVLTreeNode<T> k1) {
    AVLTreeNode<T> k2;

    k2 = k1.right;
    k1.right = k2.left;
    k2.left = k1;

    k1.height = max( height(k1.left), height(k1.right)) + 1;
    k2.height = max( height(k2.right), k1.height) + 1;

    return k2;
}

```

LR失去平衡的情况，需要经过两次旋转才能让AVL树恢复平衡：先针对局部（root的左子树）进行一次 RR单旋转（将旋转节点变为左子节点）， 此时 整棵树转换为了 LL 的情况 ， 再进行 LL单旋转

![alt](Snipaste_2018-11-30_12-47-31.png)

```java
private AVLTreeNode<T> leftRightRotation(AVLTreeNode<T> k3) {
    k3.left = rightRightRotation(k3.left);

    return leftLeftRotation(k3);
}

```

RL是与LR的对称情况

![alt](Snipaste_2018-11-30_12-50-02.png)

```java
private AVLTreeNode<T> rightLeftRotation(AVLTreeNode<T> k1) {
    k1.right = leftLeftRotation(k1.right);

    return rightRightRotation(k1);
}

```

全部的代码如下：

```java
/**
 * 自平衡二叉树
 *
 * @author xiaoyu
 * @since 1.0
 */
public class AVLTree<T extends Comparable<T>> {

    private Node<T> root;

    private class Node<T extends Comparable> {// 没有 parent 成员
        private T       value;
        private Node<T> left;
        private Node<T> right;
        private int     height;

        public Node(T value, Node<T> left, Node<T> right) {
            this.value = value;
            this.left = left;
            this.right = right;
            this.height = 0;
        }
    }

    /**
     * LL：
     * 根节点的左子树的左子树还有非空子节点 ( x 有子节点, 造成左边偏重)
     * <pre>
     *         rn                    l
     *       /  \                  /  \
     *      l    r     -->        x    rn
     *    /  \                   /\   / \
     *   x    y                      y   r
     *  /\
     * </pre>
     *
     * @param rotationNode 围绕旋转的节点
     * @return 旋转后的根节点
     */
    public Node<T> leftLeftRotation(Node<T> rotationNode) {
        Node<T> leftToTop = rotationNode.left;
        rotationNode.left = leftToTop.right;
        leftToTop.right = rotationNode;

        rotationNode.height = max(height(rotationNode.left), height(rotationNode.right)) + 1;
        leftToTop.height = max(height(leftToTop.left), height(leftToTop.right)) + 1;

        return leftToTop;
    }

    /**
     * RR:
     * 根节点的右子树的右子树还有非空子节点(y 有子节点, 造成右边偏重)
     *
     * <pre>
     *         rn                     r
     *        /  \                  /  \
     *      l    r     -->        rn    y
     *          / \              / \   /\
     *         x   y            l   x
     *            /\
     * </pre>
     *
     * @param rotationNode
     * @return
     */
    public Node<T> rightRightRotation(Node<T> rotationNode) {
        Node<T> rightToTop = rotationNode.right;
        rotationNode.right = rightToTop.left;
        rightToTop.left = rotationNode;

        rotationNode.height = max(height(rotationNode.left), height(rotationNode.right)) + 1;
        rightToTop.height = max(height(rightToTop.left), height(rightToTop.right)) + 1;

        return rightToTop;
    }

    /**
     * LR:
     * 根节点的左子树的右子树还有非空子节点
     * 第一次旋转 - RR - 通过局部 RR旋转, 转化为完全的 左左类型
     * 第二次旋转 - LL - LL 旋转
     *
     * <pre>
     *         rn                        rn                          y
     *       /  \                      /  \                        /  \
     *      l    r     -- RR ->       y    r   -- LL -->          l    rn
     *    /  \                      /  \                        / \   / \
     *   x    y                    l    w                      x  z  w   r
     *       / \                 /  \
     *      z   w               x   z
     * </pre>
     *
     * @param rotationNode
     * @return
     */
    public Node<T> leftRightRotation(Node<T> rotationNode) {
        rotationNode.left = this.rightRightRotation(rotationNode.left);
        return leftLeftRotation(rotationNode);
    }

    /**
     * RL:
     * 和 LR 相反
     * @param rotationNode
     * @return
     */
    public Node<T> rightLeftRotation(Node<T> rotationNode) {
        rotationNode.right = this.leftLeftRotation(rotationNode.right);
        return rightRightRotation(rotationNode);
    }

    // ----------------------------------------------------

    /**
     * 插入
     * @param enterNode 根节点
     * @param value 插入值
     * @return
     */
    private Node<T> insert(Node<T> enterNode, T value) {
        if (enterNode == null) {
            enterNode = new Node<>(value, null, null);
        } else {
            int compareValue = value.compareTo(enterNode.value);
            if (compareValue < 0) { // 将 value 插入到 左子树
                enterNode.left = insert(enterNode.left, value);
                // 插入后， 如果失去平衡， 需要旋转
                if (height(enterNode.left) - height(enterNode.right) >= 2) {
                    if (value.compareTo(enterNode.left.value) < 0) {
                        enterNode = leftLeftRotation(enterNode);
                    } else {
                        enterNode = leftRightRotation(enterNode);
                    }
                }
            } else if (compareValue > 0) {// 将value 插入到右子树
                enterNode.right = insert(enterNode.right, value);
                if (height(enterNode.right) - height(enterNode.left) >= 2) {
                    if (value.compareTo(enterNode.right.value) > 0) {
                        enterNode = rightRightRotation(enterNode);
                    } else {
                        enterNode = rightLeftRotation(enterNode);
                    }
                }
            } else {
                throw new RuntimeException("insert failed: cannot insert repeating value");
            }

            //while (true) {
            //    int compareValue = value.compareTo(enterNode.value);
            //    if (compareValue < 0) {
            //        enterNode = enterNode.left;
            //    } else if (compareValue > 0) {
            //        enterNode = enterNode.right;
            //    } else {
            //        throw new RuntimeException("insert failed: cannot insert repeating value");
            //    }
            //    if (enterNode == null) {
            //        break;
            //    }
            //}
        }

        enterNode.height = max(height(enterNode.left), height(enterNode.right)) + 1;
        return enterNode;

    }

    public void insert(T value) {
        root = insert(root, value);
    }

    // ---------------------------------------------------

    /**
     * 删除
     * @param enterNode 根节点
     * @param delete 待删除节点
     * @return 删除操作完成后的根节点
     */
    public Node<T> remove(Node<T> enterNode, Node<T> delete) {
        int compareValue = delete.value.compareTo(enterNode.value);
        if (compareValue < 0) {// 待删除的节点在"tree的左子树"中， 删除后， 右边偏重
            // 进入点往左边移动
            // 删除完后， 原始 enterNode的左指针指向删除完毕后的根节点
            enterNode.left = remove(enterNode.left, delete);

            if (height(enterNode.right) - height(enterNode.left) >= 2) {
                Node<T> r = enterNode.right;
                if (height(r.left) < height(r.right)) {
                    enterNode = rightRightRotation(enterNode);
                } else  {
                    enterNode = rightLeftRotation(enterNode);
                }
            }
        } else if (compareValue > 0) {// 待删除的节点在"tree的右子树"中, 删除后， 左边偏重

            enterNode.right = remove(enterNode.right, delete);

            if (height(enterNode.left) - height(enterNode.right) >= 2) {
                Node<T> l = enterNode.left;
                if (height(l.left) > height(l.right)) {
                    enterNode = leftLeftRotation(enterNode);
                }
                else {
                    enterNode = leftRightRotation(enterNode);
                }
            }

        } else {// enterNode 正是要删除的节点

            if (enterNode.left != null && enterNode.right != null) {// 左右子树都不为空
                if (height(enterNode.left) > height(enterNode.right)) {// 若左子树更高
                    // 找到左子树的最大节点 (enterNode 的 前驱), 代替 enterNode
                    // 此时, AVL树仍然是平衡的
                    Node<T> leftMax = maxNode(enterNode.left);
                    enterNode.value = leftMax.value;
                    enterNode.left = remove(enterNode.left, leftMax);
                } else {// 若右子树更高
                    // 找得到右子树的最小节点(enterNode 的后继), 代替 enterNode
                    Node<T> rightMin = minNode(enterNode.right);
                    enterNode.value = rightMin.value;
                    enterNode.right = remove(enterNode.right, rightMin);
                }
            } else if (enterNode.left == null && enterNode.right == null) {// 没有子树
                // 直接删除
                enterNode = null;
            } else {// 只有一个子树(有一个子树为空)
                // 子节点代替enterNode
                if (enterNode.right != null) {
                    enterNode = enterNode.right;
                } else {
                    enterNode = enterNode.left;
                }
            }
        }

        return enterNode;
    }

    public void remove(T value) {
        Node<T> find = find(root, value);
        if (find == null) {
            throw new RuntimeException("delete error: value ["+value+"] is not exist.");
        }
        root = remove(root, find);
    }

    // ----------------------------------------------------

    /**
     * find
     * @param enterNode 根节点
     * @param value 查找值
     * @return 查找到的节点
     */
    public Node<T> find(Node<T> enterNode, T value) {
        int compare = value.compareTo(enterNode.value);
        if (compare < 0) {
            enterNode =  find(enterNode.left, value);
        } else if (compare > 0) {
            enterNode = find(enterNode.right, value);
        }
        return enterNode;
    }


    private Node<T> maxNode(Node<T> enterNode) {
        Node<T> right = enterNode.right;
        if (right != null) {
            enterNode = maxNode(right);
        }
        return enterNode;
    }

    private Node<T> minNode(Node<T> enterNode) {
        Node<T> left = enterNode.left;
        if (left != null) {
            enterNode = minNode(left);
        }
        return enterNode;
    }

    private int max(int h1, int h2) {
        if (h1 > h2) {
            return h1;
        }
        return h2;
    }

    private int height(Node<T> node) {
        if (node != null) {
            return node.height;
        }
        return 0;
    }

    private int height() {
        return height(root);
    }
}
```

#### 红黑树

https://zhuanlan.zhihu.com/p/24367771

R-B Tree，全称是`Red-Black Tree`，又称为“红黑树”，它一种自平衡的二叉查找树。红黑树的每个节点上都有存储位表示节点的颜色，可以是红(Red)或黑(Black)。

AVL树由于实现比较复杂，而且插入和删除性能差，在实际环境下的应用不如红黑树

基于 2-3树 实现, 红黑树中将节点之间的链接分为两种不同类型，`红色链接`，他用来链接两个2-nodes节点来表示一个3-nodes节点, 并且向左倾斜，即一个2-node是另一个2-node的左子节点。`黑色链接`用来链接普通的2-3节点

![alt](Snipaste_2019-03-17_21-39-40.png)

如果将红色连接水平绘制, 会更形象化:

![alt](Snipaste_2018-11-30_16-22-45.png)

有这么几条规则需要遵守: (如果被新插入的节点打破, 就需要变色, 旋转...)

- 节点是红色或黑色。
- 根是黑色
- 所有叶子都是黑色（这里的叶子是指NIL节点, 即 null 节点）
- 如果一个节点是红色的，则它的子节点必须是黑色的。(即：从每个叶子到根的所有路径上不能有两个连续的红色节点。)
- 从任一节点到其每个叶子的所有路径都包含相同数目的黑色节点。 - 这一条确保没有一条路径会比其他路径长出俩倍。因而，红黑树是相对是接近平衡的二叉树。

这些约束确保了红黑树的关键特性: `从根到叶子的最长的可能路径不多于最短的可能路径的两倍长`

它的时间复杂度是`O(lgn)`，效率非常之高。

红黑树的应用比较广泛，主要是用它来存储有序的数据，例如，Java集合中的TreeSet和TreeMap, jdk1.8 中的hashmap

![alt](Snipaste_2018-11-30_13-18-49.png)

相对于 avl，红黑树的插入/删除效率更高， 因为允许存在部分不平衡， 减少了旋转次数 - 最多只需要 3 次旋转

相应的， 由于 avl 遵循严格的平衡， 查找效率要高一些， 但是差别不大

所以简单说，如果你的应用中，搜索的次数远远大于插入和删除，那么选择AVL，如果搜索，插入删除次数几乎差不多，应该选择RB。

`左旋和右旋`：左旋 和 右旋 是相对的两个概念，对x进行左旋，意味着"将x变成一个左节点"; 通常左旋操作用于将一个向右倾斜的红色链接旋转为向左倾斜。对比操作前后，可以看出，该操作实际上是将红线链接的两个节点中的一个较大的节点移动到根节点上

![alt](Snipaste_2018-11-30_16-25-52.png)

`颜色反转`:当出现一个临时的4-node的时候，即一个节点的两个子节点均为红色; 这其实是个A，E，S 4-node连接，我们需要将E提升至父节点，操作方法很简单，就是把E对子节点的连线设置为黑色，自己的颜色设置为红色

![alt](Snipaste_2018-11-30_16-29-02.png)

或者这么理解: 新插入的节点都当作是红色的，如果遇到父节点的颜色为黑则修复操作结束, 如果父节点为红色节点的时候是需要插入修复操作的。

todo： http://www.cnblogs.com/skywang12345/p/3603935.html

## 多路平衡查找树

### 2-3树(二三树)

每个节点允许保存一个值或者两个值, 对于普通的 2-node, 保存一个值和左右子节点的指针, 对于 3-node, 保存 2 个值和左中右 3 个指针, 两个值左小右大

![alt](Snipaste_2018-11-30_16-15-37.png)

2-3查找树实现起来比较复杂，在某些情况插入后的平衡操作可能会使得效率降低。在2-3查找树基础上改进的红黑树不仅具有较高的效率，并且实现起来较2-3查找树简单

红黑树和B树都是基于它而来

### B树(B-tree)

#### b树的概念

B树也是一种用于查找的平衡树，但是它不是二叉树。`可以拥有多于2个子节点`, 能够用来存储排序后的数据, 为`优化大块数据的读和写操作`而生

数据库索引技术里大量使用者B树和B+树的数据结构

了解b-tree的生长/添加过程, 就能推导出这些特性: https://zhuanlan.zhihu.com/p/24309634

B-tree和二叉树的一个显著的区别就是，`B-tree是从下往上生长，而二叉树是从上往下生长的`。

#### b树的特征

规则:

```java
- 子节点数量最少 2 个, 最多无穷个; 每个节点的值(关键字)数量可以有多个, 按递增次序排列，并遵循左小右大原则

- 非根节点关键字数量大于等于ceil(m/2)-1且小于等于m-1个；（注：ceil()是个朝正无穷方向取整的函数 如ceil(1.1)结果为2, m表示节点的最大分支个数)

- 非叶节点有N个子节点，则该节点的关键字数等于N-1;

- 所有叶子节点均在同一层

- 叶子节点也有指向下层的指针, , 只不过指向的是 null

```

![alt](Snipaste_2018-11-29_16-36-24.png)

查询算法: 比如要查找的值为 v

- 如果节点的关键字只有一个, 比如 根节点, 这时进入左右分支, 和二叉查找算法一样
- 如果节点关键字有多个, 那么根据大小找到 v 在 这些值中的相对位置, 根据位置决定进入哪个子节点

```java
如上图我要从上图中找到E字母，查找流程如下

（1）获取根节点的关键字进行比较，当前根节点关键字为M，E要小于M（26个字母顺序），所以往找到指向左边的子节点

（2）拿到关键字D和G，D<E<G 所以直接找到D和G中间的节点；

（3）拿到E和F，因为E=E 所以直接返回关键字和指针信息（如果树结构里面没有包含所要查找的节点则返回null）；

```

插入算法: 生长过程是从下往上的

- 先确定是几阶树, 以确定每个节点关键字个数范围, 比如: 当前是要组成一个5路查找树，那么此时m=5,关键字数必须大于等于ceil（5/2） -1小于等于5-1（关键字数小于cei(5/2) -1就要进行节点合并，大于5-1就要进行节点拆分,非根节点关键字数>=2）
- 满足节点本身比左边节点大，比右边节点小, 同个节点内的关键字也是左边小 右边大

要把3、8、31、11、23、29、50、28 这些数字构建出一个5阶树出来;

- 当前是要组成一个5路查找树，那么此时m=5,关键字数必须大于等于ceil（5/2） -1, 即 2, 小于等于5-1, 即 4;（关键字数小于cei(5/2) -1就要进行节点合并，大于5-1就要进行节点拆分)

- 先插入 3、8、31、11, 此时达到节点最大关键字个数, 再插入需要拆分, 拆分规则是: 将靠近中间的关键字提取出来作为父节点, 左右两边的构成两个子节点, 继续插入到子节点中, 超过范围就提升到父节点中...

删除算法:

- 依然是根据树的阶数计算出每个节点的关键字个数范围
- 删除关键字, 删除后, 如果当前节点关键字个数小于 ceil（m/2） -1, 就从 父节点拉一个关键字下来, 相应的要从其他节点给父节点补一个关键字

![alt](Snipaste_2018-11-29_17-13-14.png)

### B+tree(B加树)

#### b+tree定义

B+树是B树的一个升级版，也是一种多路搜索树, 相对于B树来说B+树更充分的利用了节点的空间，比b-tree 查询性能更好，其速度完全接近于二分法查找

卫星数据：指的是节点元素指向的实际数据（比如数据库中的某一行记录， 在 b-tree 中所有节点都有卫星数据， 即都带有真实数据， b+tree 中只有 leaf node 有卫星数据）

- 有k个子树的中间节点包含有k个元素（B树中是k-1个元素， 存疑？），每个元素不保存数据，只用来索引，`所有数据都保存在叶子节点`
- `叶子结点本身依关键字的大小自小而大顺序链接`, 形成有序链表
- 所有的`中间节点元素都重复存在于子节点`，在子节点元素中都是最大（或都是最小）元素
- 最大元素再根节点中

![alt](Snipaste_2019-01-13_12-40-04.png)

#### 比b-tree好的点

- 【树的层级更少， 磁盘io更少速度更快】 - 非叶子节点不保存关键字记录的指针，只进行数据索引，这样使得B+树`每个非叶子节点所能保存的关键字大大增加, 树的层级更少`所以查询数据更快
- 【查询速度更稳定】 - 叶子节点保存了父节点的所有关键字记录的指针，所有`数据地址必须要到叶子节点才能获取到`，所以每次数据查询的次数都一样, `查询速度更稳定`
- 【具备排序功能， 范围查询更简单】 - 每个叶子节点的关键字从小到大构成有序链表, `左边结尾数据都会保存右边节点开始数据的指针`, 在查询大小区间的数据时候更方便
- 【全节点遍历更快】 - `遍历整棵树只需要遍历所有的叶子节点即可`，而不需要像B树一样需要对每一层进行遍历。`有利于数据库做全表扫描`
- `不如 b树的地方: 如果经常访问的数据离根节点很近, b+树仍然需要在 叶子节点才能查到, 而 b 树 不必遍历到 叶子节点`

### B*树

B*树是B+树的变体, 不同之处:

- 首先是关键字个数限制问题，B+树初始化的关键字初始化个数是cei(m/2)，b* 树的初始化个数为（cei(2/3*m)）
- B+树节点满时就会分裂，而B*树节点满时会检查兄弟节点是否满（因为每个节点都有指向兄弟的指针），如果兄弟节点未满则向兄弟节点转移关键字，如果兄弟节点已满，则从当前节点和兄弟节点各拿出1/3的数据创建一个新的节点出来

### 前缀树(Trie树/字典树)

Tire树称为字典树，又称单词查找树

前缀树(Prefix Trees或者Trie)与树类似, 不同的是, 前缀树每个节点存储的是一个字符, 相同字符不重复存储，用于处理字符串相关的问题时非常高效。

场景: 它可以实现快速检索，常用于字典中的单词查询，搜索引擎的自动补全甚至IP路由。保存大量字符串

它的优点是：`利用字符串的公共前缀来减少查询时间`，最大限度地减少无谓的字符串比较，查询效率比哈希树高。　

下图展示了“top”, “thus”和“their”三个单词在前缀树中如何存储的：

![alt](Snipaste_2018-11-25_11-21-09.png)

应用示例:

- 串的快速检索: 给出N个单词组成的熟词表，以及一篇全用小写英文书写的文章，请你按最早出现的顺序写出所有不在熟词表中的生词。

  我们可以用数组枚举，用哈希，用字典树，先把熟词建一棵树，然后读入文章进行比较，这种方法效率是比较高的。

- 串的排序: 给定N个互不相同的仅由一个单词构成的英文名，让你将他们按字典序从小到大输出。

  用字典树进行排序，采用数组的方式创建字典树，这棵树的每个结点的所有儿子很显然地按照其字母大小排序。对这棵树进行先序遍历即可

- 最长公共前缀

  对所有串建立字典树，对于两个串的最长公共前缀的长度即他们所在的结点的公共祖先个数，于是，问题就转化为求公共祖先的问题。

## 哈夫曼树(Huffman Tree)

http://www.cnblogs.com/skywang12345/p/3706833.html

# 堆

## 二叉堆

二叉堆就是完全二叉树, 

但是排序规则不同, 按照数据的排列方式可以分为两种：最大堆和最小堆

-   大顶堆：父结点的键值总是大于或等于任何一个子节点的键值；
-   小顶堆：父结点的键值总是小于或等于任何一个子节点的键值。

![alt](Snipaste_2018-11-30_15-59-32.png)

性质: 不保证节点间的有序, 只能保证最大堆的根节点为最大值, 最小堆得根节点是最小值

使用场景:

- 优先队列 - 优先队列中的元素都有一个优先级, 最高的元素最先被取出, 队列结构可以用堆实现
- 堆排序 - 找最值得时候可以用堆结构


为了节省空间, 所以二叉堆一般都通过"数组"来存储, 数组下标规律: (假设 其中某一个元素的index假设为n, 数组下标从 1 开始)

1）它的父节点若存在，父节点的index为n//2(n//2指n除以2取整数)

2）若是左子节点存在，index为2*n

3）若是右子节点存在，index为2*n+1


## 堆排序



# 图

## 图的概念

图(graph)由多个节点(vertex)构成，节点之间可以互相连接组成一个网络。(x, y)表示一条边(edge)，它表示节点x与y相连。边可能会有权值(weight/cost)。

根据边是否有方向，将图可以划分为：无向图和有向图。


遍历算法: 广度优先搜索(Breadth First Search), 深度优先搜索(Depth First Search)


# 哈希表(hash-table)

- 哈希(Hash): 将某个对象通过哈希函数变换为唯一标识符(hash)，该标识符通常用一个短的随机字母和数字组成的字符串来代表。哈希可以用来实现各种数据结构，其中最常用的就是哈希表(hash table)。

- 哈希函数: 使用哈希函数将键映射成索引。这种映射函数就是哈希函数; 

    - 对于正整数作为键, 一般用这个整数, 除以数组的容量, 取余数, 结果作为保存到数组中的索引.

    - 字符串作为键的时候, 可以将组成字符串的每一个字符取值然后以一定规则进行哈希

- 哈希表就是一种以 键-值(key-indexed) 存储数据的结构，我们只要输入待查找的值即key，即可查找到其对应的值; 哈希表通常由数组实现。数组的下标即为哈希值，由哈希函数计算，作为哈希表的键(key)，而数组中保存的数据即为值(value)

    - 原理: 如果所有的键都是整数，那么就可以使用一个简单的无序数组来实现：将键作为索引，值即为其对应的值，这样就可以快速访问任意键的值; 如果键是复杂类型, 使用哈希函数将被查找的键转换为数组的索引, 然后根据索引到数组中查找对应的值; 

    - 碰撞冲突: 在理想的情况下，不同的键会被转换为不同的索引值，但是在有些情况下需要处理多个键被哈希到同一个索引值的情况, 怎么解决: 拉链法和线性探测法

        - 拉链法: 使得数组的每一个元素指向一个条链表, 发生碰撞的值都存储到链表中; 原则: 选择容量足够大的数组，使得所有的链表都尽可能的短小，以保证查找的效率

        - 线性探测法: 使用大小为M的数组来保存N个键值对，其中M>N，需要使用数组中的空位解决碰撞冲突; 如果发生碰撞, 就将索引+1, 值存放到相邻的下一个位置

    - 影响hash table性能的指标:

        - 哈希函数
        - 哈希表的大小
        - 哈希冲突处理方式



# 排序算法

## 选择排序(select Sort)

```java
Function selectionSort(arry)
  For i = 0 To end of arry

    smallestIndex = i
    For j = i+1 To end of array
      If array item j < array item smallestIndex
        smallestIndex = j
      End If
    Next

    swap array item at i and smallestIndex

  Next

return arry
```

复杂度分析： for循环套for循环， N*N -> O(N^2)

## 归并排序(merge sort)

```

```

复杂度: O(n*log n)

## 冒泡排序(bubble sort)

```
Function bubbleSort(array)
  For i = 0 To end of array

    swappedFlag = false
    For j = 0 To array size - i-1
      If array item j > array item j+1
        swap array item at j and j+1
        swappedFlag = true
      End If
    Next

    // 如果 没有发生交换， 说明已经是有序的了， 可以直接跳出
    If Not swappedFlag
      Break
    Eng If

  Next

return array

```

复杂度： O(N^2), 最好情况O(N)

# 查找算法

在表中执行查找动作, 分为

- static search table (静态查找表), 在表中只是做查找操作

- dynamic search table(动态查找表), 在查找过程中, 同时插入新的数据记录 或者删除某些数据记录

## 顺序查找

就是遍历查找, 不要求表有序

时间复杂度: O(n)

## 有序表查找

二分查找

要求表有序, 每次取中间坐标, 循环比较

O(logn)

## 线性索引查找

# 索引

索引是一种加快查找速度而设计的数据结构, 

一个索引包括多个索引项, 每个索引项包括关键字 and 数据在磁盘中的位置

索引就是把一个关键字和对应的 record 关联的过程

## 线性索引

### 稠密索引

### 分块索引

### 倒排索引

## 树形索引

## 多级索引

