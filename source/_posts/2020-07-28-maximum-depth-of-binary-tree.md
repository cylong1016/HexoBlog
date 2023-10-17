---
title: 二叉树的最大深度
date: 2020-07-28 18:40:03
updated: 2020-07-28 18:40:03
categories:
    - LeetCode
tags:
    - LeetCode
    - Java
    - 学习笔记
    - 树
    - 二叉树
    - 队列
    - 递归
    - 深度优先搜索
    - 广度优先搜索
---
---

# 题目描述

给定一个二叉树，找出其最大深度。二叉树的深度为根节点到最远叶子节点的最长路径上的节点数。

**说明:** 叶子节点是指没有子节点的节点。

**示例：**
```
给定二叉树 [3, 9, 20, null, null, 15, 7]，

    3
   / \
  9  20
    /  \
   15   7
返回它的最大深度 3 。
```

<!-- more -->

# 深度优先搜索

我们可以发现，给定一个二叉树t，它的最大深度是左子树l和右子树r的最大深度中的较大值加一，针对左子树和右子树，同样进行递归处理。递归的终止条件是访问到空节点，此时返回深度0。

```java
public int maxDepth(TreeNode root) {
    if (root == null) {
        return 0;
    }
    return Math.max(maxDepth(root.left), maxDepth(root.right)) + 1;
}

public class TreeNode {
    int val;
    TreeNode left;
    TreeNode right;

    TreeNode(int x) {
        val = x;
    }
}
```

## 复杂度分析

* 时间复杂度：O(n)，其中 n 为二叉树节点的个数。每个节点在递归中只被遍历一次。
* 空间复杂度：O(h)，其中 h 表示二叉树的高度。递归函数需要栈空间，而栈空间取决于递归的深度，因此空间复杂度等价于二叉树的高度。

# 广度优先搜索

我们也可以用「广度优先搜索」的方法来解决这道题目，但我们需要对其进行一些修改，此时我们广度优先搜索的队列里存放的是「当前层的所有节点」。每次拓展下一层的时候，不同于广度优先搜索的每次只从队列里拿出一个节点，我们需要将队列里的所有节点都拿出来进行拓展，这样能保证每次拓展完的时候队列里存放的是当前层的所有节点，即我们是一层一层地进行拓展，最后我们用一个变量 ans 来维护拓展的次数，该二叉树的最大深度即为 ans。

```java
public int maxDepth(TreeNode root) {
    if (root == null) {
        return 0;
    }
    Queue<TreeNode> queue = new LinkedList<TreeNode>();
    queue.offer(root);
    int ans = 0;
    while (!queue.isEmpty()) {
        int size = queue.size();
        while (size > 0) {
            TreeNode node = queue.poll();
            if (node.left != null) {
                queue.offer(node.left);
            }
            if (node.right != null) {
                queue.offer(node.right);
            }
            size--;
        }
        ans++;
    }
    return ans;
}

public class TreeNode {
    int val;
    TreeNode left;
    TreeNode right;

    TreeNode(int x) {
        val = x;
    }
}
```

## 复杂度分析

* 时间复杂度：O(n)，其中 n 为二叉树节点的个数。每个节点只会被访问一次。
* 空间复杂度：O(n)，此方法空间的消耗取决于队列存储的元素数量，其在最坏情况下会达到 O(n)。

# 来源

> [二叉树的最大深度 | 力扣（LeetCode）][1]
> [二叉树的最大深度 | 题解（LeetCode）][2]

---

[1]: https://leetcode-cn.com/problems/maximum-depth-of-binary-tree/ "二叉树的最大深度 | 力扣（LeetCode）"
[2]: https://leetcode-cn.com/problems/maximum-depth-of-binary-tree/solution/er-cha-shu-de-zui-da-shen-du-by-leetcode-solution/ "二叉树的最大深度 | 题解（LeetCode）"
