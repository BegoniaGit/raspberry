# 从认识到实现Skip List

跳跃表

## 前言

增加了向前指针的链表叫作跳表。跳表全称叫做跳跃表，简称跳表。跳表是一个随机化的数据结构，实质就是一种可以进行二分查找的有序链表。跳表在原有的有序链表上面增加了多级索引，通过索引来实现快速查找。跳表不仅能提高搜索性能，同时也可以提高插入和删除操作的性能。

### 1. 跳表的样儿

![&#x8DF3;&#x8868;&#x7ED3;&#x6784;&#x56FE;](https://shaosim-image.oss-cn-chengdu.aliyuncs.com/跳表结构图.jpg)

### 2. 跳表具有如下性质：

1. 由很多层结构组成。
2. 每一层都是一个有序的链表。
3. 最底层\(第一层\)的链表包含所有元素 。
4. 如果一个元素出现在 第 i 层 的链表中，则它在第 i 层 之下的链表也都会出现。 
5. 每个节点包含两个指针，一个指向同一链表中的下一个元素，一个指向下面一层的元素。

### 3. 来一个栗子,你将豁然开朗

栗子：查找元素 6 

1. 头指针最开始在第一层最小值INT\_MIN处 
2. 于是和旁边的1比较,当然了1大嘛,于是指针移向1 
3. 再和当前层4比较， 比 4 大，跳到4 
4. 与7比较,比7小,于是向下跳到第二层4 
5. 与5比较,比5大,跳到5 
6. 与7比较,比7小,跳到最低层5 
7. 只能依次向后比较了,于是找到了6

### 4. 笔者的Java实现,慢慢看,很清晰

1. 节点定义,共三个成员变量:value,right,down

```java
package crabapple;

/*
 * Copyright (c) This is zhaoxubin's Java program.
 * Copyright belongs to the crabapple organization.
 * The crabapple organization has all rights to this program.
 * No individual or organization can refer to or reproduce this program without permission.
 * If you need to reprint or quote, please post it to zhaoxubin2016@live.com.
 * You will get a reply within a week,
 *
 */

import java.util.Random;

/**
 * 节点类定义
 */
class Node {

    //值
    public int value = 0;

    //当前层下一个节点
    public Node right;

    //下一层,直连的节点
    public Node down;

    //构造函数
    public Node() {

    }

    public Node(int value) {
        this.value = value;
    }
}
```

2. 跳表类 定义,大家可以清晰的看请代码逻辑结构,该类只暴露insert\(\)和search\(\)两个方法,其它变量及方法均设为私有.

```java
/**
 * 跳表定义
 */
public class SkipTable {

    //表层数
    private int levelCount;

    //表的头指针
    private Node firstNode;

    //初始化:层数为1,共前后两个节点,一个最小值,一个最大值
    private void init() {
        levelCount = 1;
        firstNode=new Node();
        firstNode.value = Integer.MIN_VALUE;
        firstNode.right = new Node(Integer.MAX_VALUE);
    }

    public SkipTable() {
        init();
    }

    /**
     * 查找值
     * @param value
     * @return
     */
    public boolean search(int value) {

        Node current = firstNode;
        return toSearch(current, value);
    }

    private boolean toSearch(Node node, int value) {
        if (node.value == value)
            return true;
        else if (node.right!=null&&value >= node.right.value)
            return toSearch(node.right, value);
        else if (node.down != null)
            return toSearch(node.down, value);
        return false;
    }



    /**
     * 插入值
     * @param value
     * @return
     */
    public boolean insert(int value) {

        //判断是否有这个元素
        if (search(value))
            return false;

        //随机获取一个层数
        int willLevel = updateLevelCount();

        //判断是否添加新层
        if (willLevel > levelCount) {
            Node newFirstNode = new Node();
            addLevel(firstNode, newFirstNode);
            firstNode=newFirstNode;
            levelCount = willLevel;
        }

        //插入新元素
        Node port = firstNode;
        int skipLevel = levelCount - willLevel;

        //迭代到指定层
        while ((skipLevel--) > 0)
            port = port.down;

        //上下层新节点的桥梁
        Node insertNode = null;

        while (port != null) {

            //获取当前层第一个节点指针
            Node curPort = port;

            //迭代到右边的节点值比自己大为止
            while (port.right.value < value)
                port = port.right;

            //准备插入的新节点
            Node curInNode = new Node(value);

            //更新当前节点和前节点指针指向
            curInNode.right = port.right;
            port.right = curInNode;

            //将当前节点引用给上层节点
            if (insertNode != null)
                insertNode.down = curInNode;

            //将新插入的节点指针更新到insertNode,以备在下一层建立指向
            insertNode = curInNode;

            //行头指针向下迭代
            port = port.down;
        }
        return true;

    }

    /**
     * 添加新层
     *
     * @param oldFirst
     * @param newFirst
     */
    private void addLevel(Node oldFirst, Node newFirst) {

        newFirst.value = oldFirst.value;
        newFirst.down = oldFirst;
        if (oldFirst.right != null) {
            Node newRightNode = new Node();
            newFirst.right = newRightNode;
            addLevel(oldFirst.right, newRightNode);
        }
    }

    /**
     * 以一定概率使得获取一个和老层数差值不超过1的新层数
     * @return
     */
    private int updateLevelCount() {

        Random random=new Random();
        int v = random.nextInt(10);
        return v ==0 ? random.nextInt(levelCount) + 2 : random.nextInt(levelCount)+1 ;
    }
}
```

3. 测试类

```java
package crabapple;


public class Main {

    public static void main(String[] args) {
        //测试
        SkipTable skipTable=new SkipTable();
        skipTable.insert(1);
        skipTable.insert(2);
        skipTable.insert(3);
        skipTable.insert(4);
        skipTable.insert(5);
        skipTable.insert(6);
        skipTable.insert(7);
        skipTable.insert(8);
        skipTable.insert(9);
        skipTable.insert(10);

        //边界值测试
        System.out.println(skipTable.search(0));
        System.out.println(skipTable.search(1));
        System.out.println(skipTable.search(2));
        System.out.println(skipTable.search(5));
        System.out.println(skipTable.search(9));
        System.out.println(skipTable.search(10));
        System.out.println(skipTable.search(11));
    }
}

//output:
/**
 * false
 * true
 * true
 * true
 * true
 * true
 * false
 *
 * Process finished with exit code 0
 */
```

## 结语

上面的代码,大家是可以直接运行,笔者能力有限,代码也许还有不足,望大家多多指教.

