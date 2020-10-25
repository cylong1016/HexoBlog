---
title: 填充每个节点的下一个右侧节点指针
date: 2020-10-15 18:46:48
updated: 2020-10-15 18:46:48
categories:
    - LeetCode
tags:
    - leetcode
    - java
    - 学习笔记
    - 树
    - 二叉树
    - 递归
    - 广度优先搜索
    - 深度优先搜索
    - 队列
---
---

# 题目描述

给定一个完美二叉树，其所有叶子节点都在同一层，每个父节点都有两个子节点。二叉树定义如下：
```
struct Node {
  int val;
  Node *left;
  Node *right;
  Node *next;
}
```
填充它的每个 next 指针，让这个指针指向其下一个右侧节点。如果找不到下一个右侧节点，则将 next 指针设置为 NULL。初始状态下，所有 next 指针都被设置为 NULL。

**示例：**
{% asset_img 116_sample.png 完美二叉树 %}

> 输入：{"$id":"1","left":{"$id":"2","left":{"$id":"3","left":null,"next":null,"right":null,"val":4},"next":null,"right":{"$id":"4","left":null,"next":null,"right":null,"val":5},"val":2},"next":null,"right":{"$id":"5","left":{"$id":"6","left":null,"next":null,"right":null,"val":6},"next":null,"right":{"$id":"7","left":null,"next":null,"right":null,"val":7},"val":3},"val":1}
> 输出：{"$id":"1","left":{"$id":"2","left":{"$id":"3","left":null,"next":{"$id":"4","left":null,"next":{"$id":"5","left":null,"next":{"$id":"6","left":null,"next":null,"right":null,"val":7},"right":null,"val":6},"right":null,"val":5},"right":null,"val":4},"next":{"$id":"7","left":{"$ref":"5"},"next":null,"right":{"$ref":"6"},"val":3},"right":{"$ref":"4"},"val":2},"next":null,"right":{"$ref":"7"},"val":1}
> 解释：给定二叉树如图 A 所示，你的函数应该填充它的每个 next 指针，以指向其下一个右侧节点，如图 B 所示。

**提示：**

* 你只能使用常量级额外空间。
* 使用递归解题也符合要求，本题中递归程序占用的栈空间不算做额外的空间复杂度。

<!-- more -->

# 按层遍历

按层遍历是常规的思路，基本框架就是二叉树的按层遍历，这里使用两个队列 queue 记录二叉树的节点，queueLevel 记录二叉树节点所在的层。每次往 queue 队列添加节点的时候，同时记录当前节点所在的层数，遍历层的时候，如果当前节点的层等于下一个节点的层，则执行操作 node.next = nextNode 。最后层次遍历完全部节点后，即完成了填充每个节点的下一个右侧节点的操作。

```java
public Node connect(Node root) {
        if (root == null) {
            return null;
        }

        Queue<Node> queue = new LinkedList<>();
        Queue<Integer> queueLevel = new LinkedList<>();
        int curLevel = 1;
        queue.offer(root);
        queueLevel.offer(1);
        while (!queue.isEmpty()) {
            Node node = queue.poll();
            Node nextNode = queue.peek();
            Integer level = queueLevel.poll();
            Integer nextLevel = queueLevel.peek();
            if (level != null && level != curLevel) {
                curLevel = level;
            }

            if (level == nextLevel) {
                node.next = nextNode;
            }

            if (node.left != null) {
                queue.offer(node.left);
                queueLevel.offer(curLevel + 1);
            }
            if (node.right != null) {
                queue.offer(node.right);
                queueLevel.offer(curLevel + 1);
            }
        }

        return root;
    }
}

class Node {
    public int val;
    public Node left;
    public Node right;
    public Node next;

    public Node() {}
    
    public Node(int _val) {
        val = _val;
    }

    public Node(int _val, Node _left, Node _right, Node _next) {
        val = _val;
        left = _left;
        right = _right;
        next = _next;
    }
};
```

## 复杂度分析

* 时间复杂度：O(N)。每个节点会被访问一次且只会被访问一次，即从队列中弹出，并建立 next 指针。
* 空间复杂度：O(N)。这是一棵完美二叉树，它的最后一个层级包含 N/2 个节点。广度优先遍历的复杂度取决于一个层级上的最大元素数量。这种情况下空间复杂度为 O(N)。

# 使用已有的 next 指针

一棵树中，存在两种类型的 next 指针。

1. 第一种情况是连接同一个父节点的两个子节点。它们可以通过同一个节点直接访问到，因此执行下面操作即可完成连接。
```java
node.left.next = node.right
```
{% asset_img node1.png 情况1 %}
2. 第二种情况在不同父亲的子节点之间建立连接，这种情况不能直接连接。我们可以发现当前节点右节点的指针指向的是当前节点父节点 next 节点的左节点，如果父节点没有 next 节点，则指向 null。
```java
node.right.next = node.next != null ? node.next.left : null;
```
{% asset_img node2.png 情况2 %}

这里我们使用递归可以很方便的解决上述问题。
```java
public Node connect(Node root) {
    if (root == null) {
        return root;
    }
    if (root.left != null) {
        root.left.next = root.right;
        root.right.next = root.next != null ? root.next.left : null;
        connect(root.left);
        connect(root.right);
    }
    return root;
}
```

## 复杂度分析

* 时间复杂度：O(N)，每个节点只访问一次。
* 空间复杂度：O((logn)，不需要存储额外的节点。这里只有递归占用的空间，满足题目要求。

# 来源
> [填充每个节点的下一个右侧节点指针 | 力扣（LeetCode）][1]
> [填充每个节点的下一个右侧节点指针 | 题解（LeetCode）][2]

---

> 文章标题：<a href='{{ permalink }}' title='{{ title }}' >{{ title }}</a>
> 文章作者：[cylong](http://www.cylong.com/about/ "cylong")
> 文章链接：<a href='{{ permalink }}' title='{{ title }}' >{{ permalink }}</a>
> 有问题或者建议欢迎在下方评论。欢迎转载、引用，但希望标明出处，感激不尽(●'◡'●)

[1]: https://leetcode-cn.com/problems/populating-next-right-pointers-in-each-node/ "填充每个节点的下一个右侧节点指针 | 力扣（LeetCode）"
[2]: https://leetcode-cn.com/problems/populating-next-right-pointers-in-each-node/solution/tian-chong-mei-ge-jie-dian-de-xia-yi-ge-you-ce-2-4/ "填充每个节点的下一个右侧节点指针 | 题解（LeetCode）"
