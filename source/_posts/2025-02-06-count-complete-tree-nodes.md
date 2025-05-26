---
title: 完全二叉树的节点个数
date: 2025-02-06 22:10:56
updated: 2025-02-06 22:10:56
categories:
    - LeetCode
tags:
    - LeetCode
    - LeetCode简单
    - Java
    - 学习笔记
    - 二叉树
    - 完全二叉树
    - 满二叉树
    - 递归
---
---

# 题目描述

给你一棵完全二叉树的根节点 root ，求出该树的节点个数。

**说明:**
完全二叉树：除了最底层节点可能没填满外，其余每层节点数都达到最大值，并且最下面一层的节点都集中在该层最左边的若干位置。若最底层为第 h 层（从第 0 层开始），则该层包含 1 ~ 2h 个节点。
```
     1
   /   \
  2     3
 / \   /
4   5 6
```

**示例 1:**
> 输入：root = [1,2,3,4,5,6]
> 输出：6

**示例 2:**
> 输入：root = []
> 输出：0

**示例 3:**
> 输入：root = [1]
> 输出：1

**提示:**
* 树中节点的数目范围是[0, 5 * 10^4]
* 0 <= Node.val <= 5 * 10^4
* 题目数据保证输入的树是 完全二叉树

<!-- more -->

# 思路分析

如果是普通二叉树的话，最直接的方法就是递归或者迭代遍历每个节点，然后统计个数，这样时间复杂度是O(n)，其中 n 是树的节点总数。空间复杂度是O(h)，其中 h 是树的高度。

```java
public int countNodes(TreeNode root) {
    if (root == null) {
        return 0;
    }
    return 1 + countNodes(root.left) + countNodes(root.right);
}
```

但这是一个普适的解法，对于此题给的完全二叉树的特点没有利用起来，我们先了解一下满二叉树的概念：一个二叉树，如果每一个层的结点数都达到最大值，则这个二叉树就是满二叉树，满二叉树的节点总数是 (2^h) - 1 
```
     1
   /   \
  2     3
 / \   / \
4   5 6   7
```

而完全二叉树的特点是除了最后一层外，其余层都是满的，并且最后一层的节点尽可能靠左。因此，可以利用这个特性来高效计算节点数，而不是普通的遍历方法。首先计算左子树高度 left 和右子树高度 right，接下来比较 left 和 right。

* 如果两者相等，说明左子树是满的，此时左子树的节点数为2^left - 1，加上根节点，总共是2^left。然后递归计算右子树的节点数。
* 如果 left 不等于 right，则说明右子树是满的，但层数比左子树少一层，所以右子树的节点数是2^right - 1，加上根节点，再递归计算左子树的节点数。

这里的关键在于，当左右子树高度相等时，左子树一定是满的，可以快速计算其节点数，而无需递归下去；反之，右子树是满的，可以同样处理。

```java
public int countNodes(TreeNode root) {
    if (root == null) {
        return 0;
    }
    // 计算左子树高度
    int left = countLevel(root.left);
    // 计算右子树高度
    int right = countLevel(root.right);

    if (left == right) {
        // 左子树是满的，直接计算左子树节点数 (2^left)，递归计算右子树
        return countNodes(root.right) + (1 << left);
    } else {
        // 右子树是满的，直接计算右子树节点数 (2^right)，递归计算左子树
        return countNodes(root.left) + (1 << right);
    }
}

private int countLevel(TreeNode root) {
    int level = 0;
    while (root != null) {
        // 完全二叉树的高度由最左路径决定
        level++;
        root = root.left;
    }
    return level;
}
```

## 复杂度分析

* 时间复杂度：O((logN)^2)，每次递归调用 countLevel 的时间为 O(h)（h 是当前子树高度），递归深度为树的高度 O(logN)（完全二叉树高度为 logN）。
* 空间复杂度：递归深度为树的高度 O(logN)。

# 来源

> [222. 完全二叉树的节点个数 | 力扣（LeetCode）][1]

---

[1]: https://leetcode.cn/problems/count-complete-tree-nodes/description/ "222. 完全二叉树的节点个数 | 力扣（LeetCode）"
