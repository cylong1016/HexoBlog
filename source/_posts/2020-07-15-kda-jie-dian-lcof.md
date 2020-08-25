---
title: 二叉搜索树的第k大节点
date: 2020-07-15 13:03:36
updated: 2020-07-15 13:03:36
categories:
    - LeetCode
tags:
    - leetcode
    - java
    - 学习笔记
    - 剑指Offer
    - 树
    - 二叉树
    - 二叉搜索树
    - 递归
    - 中序遍历
    - 深度优先搜索
---
---

# 题目描述

给定一棵二叉搜索树，请找出其中第k大的节点。

**示例 1:**
```
输入: root = [3, 1, 4, null, 2], k = 1
   3
  / \
 1   4
  \
   2
输出: 4
```

**示例 2:**
```
输入: root = [5, 3, 6, 2, 4, null, null, 1], k = 3
       5
      / \
     3   6
    / \
   2   4
  /
 1
输出: 4
```

限制：
1 ≤ k ≤ 二叉搜索树元素个数

<!-- more -->

# 题解

二叉搜索树的中序遍历是递增序列，那么倒序就是递减序列。

> 中序遍历为“左、根、右”的顺序
> 中序遍历的倒序为“右、根、左”的顺序

```java
// 打印中序遍历
void dfs(TreeNode root) {
    if(root == null) return;
    dfs(root.left); // 左
    System.out.println(root.val); // 根
    dfs(root.right); // 右
}
```

所以我们只要求出二叉搜索树的中序遍历倒序的第k个节点即可。

**递归解析：**
* 递归的终止条件：当节点为空的时候，越过了叶子节点，则直接返回;
* 递归右子树：dfs(root.right);
* 递归操作：先进行k - 1，然后判断 k 是否等于0，是则找到第k大的节点，将节点的值返回。
* 递归左子树：dfs(root.left);

![中序遍历图解](中序遍历.png)

```java
int k;
int res;
public int kthLargest(TreeNode root, int k) {
    this.k = k;
    dfs(root);
    return res;
}

public void dfs(TreeNode t) {
    if (t == null) {
        return;
    }
    dfs(t.right);
    if (k == 0) {
        return;
    }
    if (--k == 0) {
        res = t.val;
        return;
    }
    dfs(t.left);
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

* 时间复杂度 Ο(N) ： 当树退化为链表时（全部为右子节点），无论 k 的值大小，递归深度都为 N ，占用 Ο(N) 时间。
* 空间复杂度 Ο(N) ： 当树退化为链表时（全部为右子节点），系统使用 Ο(N) 大小的栈空间。

# 来源
> [二叉搜索树的第k大节点 | 力扣（LeetCode）][1]
> [二叉搜索树的第k大节点 | 题解（LeetCode）][2]

---

> 文章标题：<a href='{{ permalink }}' title='{{ title }}' >{{ title }}</a>
> 文章作者：[cylong](http://www.cylong.com/about/ "cylong")
> 文章链接：<a href='{{ permalink }}' title='{{ title }}' >{{ permalink }}</a>
> 有问题或者建议欢迎在下方评论。欢迎转载、引用，但希望标明出处，感激不尽(●'◡'●)

[1]: https://leetcode-cn.com/problems/er-cha-sou-suo-shu-de-di-kda-jie-dian-lcof/ "二叉搜索树的第k大节点 | 力扣（LeetCode）"
[2]: https://leetcode-cn.com/problems/er-cha-sou-suo-shu-de-di-kda-jie-dian-lcof/solution/mian-shi-ti-54-er-cha-sou-suo-shu-de-di-k-da-jie-d/ "二叉搜索树的第k大节点 | 题解（LeetCode）"
