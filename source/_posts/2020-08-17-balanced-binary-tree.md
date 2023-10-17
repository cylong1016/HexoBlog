---
title: 平衡二叉树
date: 2020-08-17 15:19:04
updated: 2020-08-17 15:19:04
categories:
    - LeetCode
tags:
    - LeetCode
    - Java
    - 学习笔记
    - 递归
    - 树
    - 二叉树
    - 平衡二叉树
    - 深度优先搜索
---
---

# 题目描述

给定一个二叉树，判断它是否是高度平衡的二叉树。本题中，一棵高度平衡二叉树定义为：一个二叉树每个节点的左右两个子树的高度差的绝对值不超过1。

**示例 1:**
```
给定二叉树 [3, 9, 20, null, null, 15, 7]
    3
   / \
  9  20
    /  \
   15   7
返回 true 。
```

**示例 2:**
```
给定二叉树 [1, 2, 2, 3, 3, null, null, 4, 4]
       1
      / \
     2   2
    / \
   3   3
  / \
 4   4
返回 false 。
```

<!-- more -->

# 题解

根据题意一个二叉树每个节点的左右两个子树的高度差的绝对值不超过1。那么我们可以递归的判断一棵树的左右子树，判断是否是平衡二叉树。对于当前遍历到的节点，先递归地判断其左右子树是否平衡，再判断以当前节点为根的子树是否平衡。如果一棵子树是平衡的，则返回其高度（高度一定是非负整数），否则返回 -1。如果存在一棵子树不平衡，则整个二叉树一定不平衡。

```java
public boolean isBalanced(TreeNode root) {
    return height(root) != -1;
}

public int height(TreeNode node) {
    if (node == null) {
        return 0;
    }

    int leftHeight = height(node.left);
    int rightHeight = height(node.right);
    if (leftHeight == -1 || rightHeight == -1 || Math.abs(leftHeight - rightHeight) > 1) {
        return -1;
    } else {
        return Math.max(leftHeight, rightHeight) + 1;
    }
}
```

对于上述代码，我们还可以简单优化下，如果左子树不是平衡的，那么也就不需要再递归的求右子树是否平衡了。

```java
public boolean isBalanced(TreeNode root) {
    return height(root) != -1;
}

public int height(TreeNode node) {
    if (node == null) {
        return 0;
    }

    int leftHeight;
    int rightHeight;
    if ((leftHeight = height(node.left)) == -1
            || (rightHeight = height(node.right)) == -1
            || Math.abs(leftHeight - rightHeight) > 1) {
        return -1;
    } else {
        return Math.max(leftHeight, rightHeight) + 1;
    }
}
```

另外也可以先看一道简单的求树的高度的题：[二叉树的最大深度 | 笑话人生][3]

## 复杂度分析

* 时间复杂度：O(n)，其中 n 是二叉树中的节点个数。使用自底向上的递归，每个节点的计算高度和判断是否平衡都只需要处理一次，最坏情况下需要遍历二叉树中的所有节点，因此时间复杂度是 O(n)。
* 空间复杂度：O(n)，其中 n 是二叉树中的节点个数。空间复杂度主要取决于递归调用的层数，递归调用的层数不会超过 n。

# 来源

> [平衡二叉树 | 力扣（LeetCode）][1]
> [平衡二叉树 | 题解（LeetCode）][2]

---

[1]: https://leetcode-cn.com/problems/balanced-binary-tree/ "平衡二叉树 | 力扣（LeetCode）"
[2]: https://leetcode-cn.com/problems/balanced-binary-tree/solution/ping-heng-er-cha-shu-by-leetcode-solution/ "平衡二叉树 | 题解（LeetCode）"
[3]: /blog/2020/07/28/maximum-depth-of-binary-tree/
