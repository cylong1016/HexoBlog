---
title: 二叉树的遍历
date: 2020-09-02 22:51:37
updated: 2020-09-21 23:30:34
categories:
    - 数据结构与算法
tags:
    - leetcode
    - 数据结构与算法
    - 树
    - 二叉树
    - 递归
    - 链表
    - 栈
    - 队列
    - 深度优先搜索
    - 广度优先搜索
    - 前序遍历
    - 中序遍历
    - 后序遍历
---
---

# 前言

最近一直在刷 Leetcode ，其中涉及了很多二叉树相关的题，二叉树是一种很重要的数据结构，很多其他的数据结构都是以二叉树为基础，二叉树的遍历涉及很多种，包括前序遍历、中序遍历、后序遍历、层次遍历。开始一直分不清这些遍历是如何工作的，随着后面题刷的越来越多，也渐渐熟悉了二叉树的遍历方式，在这里做一个总结分享给大家。

四种遍历的主要方式为：

* 前序遍历：根节点 -> 左子树 -> 右子树
* 中序遍历：左子树 -> 根节点 -> 右子树
* 后序遍历：左子树 -> 右子树 -> 根节点
* 层次遍历：从上到下按照层遍历

接下来使用以下的二叉树做样例：
```

        1
       / \
      2   3
     / \   \
    4   5   6
       / \
      7   8

前序遍历：1  2  4  5  7  8  3  6 
中序遍历：4  2  7  5  8  1  3  6
后序遍历：4  7  8  5  2  6  3  1
层次遍历：1  2  3  4  5  6  7  8
```

<!-- more -->

# 前序遍历

前序遍历：根节点 -> 左子树 -> 右子树。前序遍历是先输出根节点的值，再去递归的输出左子树和右子树。代码实现也包括递归和非递归两个版本。

## 递归

树的结构定义本身就是递归的定义，所以使用递归版本实现树的遍历会使代码更加简洁易于理解。

```java
public void dfsPreOrderTraverse(TreeNode root) {
    if (root == null) {
        return;
    }
    System.out.print(root.val + " ");
    dfsPreOrderTraverse(root.left);
    dfsPreOrderTraverse(root.right);
}
```

## 非递归

非递归版本，就没有递归版本那么好理解，代码也比较多，我们这里引入栈来保存每个节点，首先将根节点加入栈中，接下来我们遍历此栈，根据栈的后进先出特性，每次将栈顶元素退出，并输出其值，接下来，我们将此节点的右节点和左节点依次加入到栈中，根据栈的后进先出特性，永远都是先输出左子树，然后输出右子树。

```java
public void preOrderTraverse(TreeNode root) {
    if (root == null) {
        return;
    }
    Stack<TreeNode> stack = new Stack<>();
    stack.push(root);
    while (!stack.isEmpty()) {
        TreeNode node = stack.pop();
        System.out.print(node.val + " ");
        if (node.right != null) {
            stack.push(node.right);
        }
        if (node.left != null) {
            stack.push(node.left);
        }
    }
}
```

# 中序遍历

中序遍历：左子树 -> 根节点 -> 右子树。中序遍历是中间输出根节点的值，先递归左子树，然后输出根节点的值，再递归右子树。

```java
public void dfsInOrderTraverse(TreeNode root) {
    if (root == null) {
        return;
    }
    dfsInOrderTraverse(root.left);
    System.out.print(root.val + " ");
    dfsInOrderTraverse(root.right);
}
```

# 后续遍历

后序遍历：左子树 -> 右子树 -> 根节点。后续遍历是先递归左子树和右子树，最后输出根节点的值。

```java
public void dfsPostOrderTraverse(TreeNode root) {
    if (root == null) {
        return;
    }
    dfsPostOrderTraverse(root.left);
    dfsPostOrderTraverse(root.right);
    System.out.print(root.val + " ");
}
```

# 层次遍历

层次遍历：从上到下按照层遍历。这里不是递归的去遍历，而是横向的遍历每一层，这里我们引入队列，我们先把根节点加入到队列中，接下来，根据队列的先进先出特性，我们先从队列中取出最先加入的节点，输出其值，然后将此节点的左右节点分别加入到队列中。

```java
public void levelTraverse(TreeNode root) {
    if (root == null) {
        return;
    }
    Queue<TreeNode> queue = new LinkedList<>();
    queue.offer(root);
    while (!queue.isEmpty()) {
        TreeNode node = queue.poll();
        System.out.print(node.val + " ");
        if (node.left != null) {
            queue.offer(node.left);
        }
        if (node.right != null) {
            queue.offer(node.right);
        }
    }
}
```

另外一种比较复杂的按层遍历是每层当成一个列表输出，这个时候我们只要增加另外一个队列，同时记录当时遍历的层数即可。

```java
public ArrayList<ArrayList<Integer>> levelTraverse(TreeNode root) {
    ArrayList<ArrayList<Integer>> ans = new ArrayList<>();
    ArrayList<Integer> item = new ArrayList<>();
    if (root == null) {
        return ans;
    }

    Queue<TreeNode> queue = new LinkedList<>();
    Queue<Integer> queueLevel = new LinkedList<>();
    int curLevel = 1;
    queue.offer(root);
    queueLevel.offer(1);
    while (!queue.isEmpty()) {
        TreeNode node = queue.poll();
        int level = queueLevel.poll();
        if (level != curLevel) {
            curLevel = level;
            ans.add(new ArrayList<>(item));
            item.clear();
        }
        item.add(node.val);

        if (node.left != null) {
            queue.offer(node.left);
            queueLevel.offer(curLevel + 1);
        }
        if (node.right != null) {
            queue.offer(node.right);
            queueLevel.offer(curLevel + 1);
        }
    }

    if (!item.isEmpty()) {
        ans.add(new ArrayList<>(item));
    }

    return ans;
}
```

输出样例：[\[1\], [2, 3], [4, 5, 6], [7, 8]]

# 总结

其实写完代码就可以发现很多有意思的事情，之前模糊不清的概念也都搞清楚了。

1. 前序、中序、后序遍历其实是针对根节点来说的，对于左右子节点，都是先左后右。另外无论是哪种遍历方式，都是先遍历（访问）根节点，区别就是什么时候处理根节点（比如输出根节点的值）。
2. 广度优先搜索对于树来说，其实就是层次遍历，深度优先搜索对于树来说，其实就是先序遍历。
3. 树的层次遍历和树的先序遍历的非递归版本，其实代码一样，只不过一个使用的是队列，一个使用的是栈。

# 参考

> [二叉树遍历（前序、中序、后序、层次遍历、深度优先、广度优先）][1]

---

> 文章标题：<a href='{{ permalink }}' title='{{ title }}' >{{ title }}</a>
> 文章作者：[cylong](http://www.cylong.com/about/ "cylong")
> 文章链接：<a href='{{ permalink }}' title='{{ title }}' >{{ permalink }}</a>
> 有问题或者建议欢迎在下方评论。欢迎转载、引用，但希望标明出处，感激不尽(●'◡'●)

[1]: https://blog.csdn.net/My_Jobs/article/details/43451187 "二叉树遍历（前序、中序、后序、层次遍历、深度优先、广度优先）"
