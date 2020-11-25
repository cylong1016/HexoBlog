---
title: 求根到叶子节点数字之和
date: 2020-11-25 22:52:29
updated: 2020-11-25 22:52:29
categories:
    - LeetCode
tags:
    - leetcode
    - java
    - 学习笔记
    - 树
    - 二叉树
    - 深度优先搜索
    - 回溯算法
    - 递归
---
---

# 题目描述

给定一个二叉树，它的每个结点都存放一个 0-9 的数字，每条从根到叶子节点的路径都代表一个数字。
例如，从根到叶子节点路径 1->2->3 代表数字 123。计算从根到叶子节点生成的所有数字之和。

**说明:** 叶子节点是指没有子节点的节点。

**示例 1:**
```
输入: [1, 2, 3]
    1
   / \
  2   3
输出: 25
解释:
从根到叶子节点路径 1->2 代表数字 12.
从根到叶子节点路径 1->3 代表数字 13.
因此，数字总和 = 12 + 13 = 25.
```

**示例 2:**
```
输入: [4, 9, 0, 5, 1]
    4
   / \
  9   0
 / \
5   1
输出: 1026
解释:
从根到叶子节点路径 4->9->5 代表数字 495.
从根到叶子节点路径 4->9->1 代表数字 491.
从根到叶子节点路径 4->0 代表数字 40.
因此，数字总和 = 495 + 491 + 40 = 1026.
```

<!-- more -->

# 深度优先搜索

此题中，每个节点都对应一个 0-9 的数字，每条从根节点到叶子节点的路径都代表一个数字。我们只要通过深度优先搜索加回溯算法，求出所有路径组成的数字，再将所有数字相加求和即可。具体的，从根节点开始，遍历每个节点，如果遇到叶子节点，则将组成的数字保存，并进行回溯。如果不是叶子节点，则递归遍历子节点构造数字。

```java
List<List<Integer>> res = new ArrayList<>();
LinkedList<Integer> item = new LinkedList<>();

public int sumNumbers(TreeNode root) {
    dfsBuildNumbers(root);
    int sum = 0;
    for (List<Integer> list : res) {
        sum += parseInt(list);
    }
    return sum;
}

public void dfsBuildNumbers(TreeNode node) {
    if (node == null) {
        return;
    }
    item.add(node.val);
    if (node.left == null && node.right == null) {
        res.add(new LinkedList<>(item));
    }
    dfsBuildNumbers(node.left);
    dfsBuildNumbers(node.right);
    item.removeLast();
}

public int parseInt(List<Integer> list) {
    int res = 0;
    for (int n : list) {
        res = res * 10 + n;
    }
    return res;
}
```

其实，每个节点都对应一个数字，等于其父节点对应的数字乘以 10 再加上该节点的值（这里假设根节点的父节点对应的数字是 0）。只要计算出每个叶子节点对应的数字，然后计算所有叶子节点对应的数字之和，即可得到结果。可以通过深度优先搜索实现。
{% asset_img number.png 图解 %}

```java
public int sumNumbers(TreeNode root) {
    return dfs(root, 0);
}

public int dfs(TreeNode root, int prevSum) {
    if (root == null) {
        return 0;
    }
    int sum = prevSum * 10 + root.val;
    if (root.left == null && root.right == null) {
        return sum;
    } else {
        return dfs(root.left, sum) + dfs(root.right, sum);
    }
}
```

## 复杂度分析

* 时间复杂度：O(n)，其中 n 是二叉树的节点个数。对每个节点访问一次。
* 空间复杂度：O(n)，其中 n 是二叉树的节点个数。空间复杂度主要取决于递归调用的栈空间，递归栈的深度等于二叉树的高度，最坏情况下，二叉树的高度等于节点个数，空间复杂度为 O(n)。

# 来源

> [求根到叶子节点数字之和 | 力扣（LeetCode）][1]
> [求根到叶子节点数字之和 | 题解（LeetCode）][2]

---

> 文章标题：<a href='{{ permalink }}' title='{{ title }}' >{{ title }}</a>
> 文章作者：[cylong](/about/ "cylong")
> 文章链接：<a href='{{ permalink }}' title='{{ title }}' >{{ permalink }}</a>
> 有问题或者建议欢迎在下方评论。欢迎转载、引用，但希望标明出处，感激不尽(●'◡'●)

[1]: https://leetcode-cn.com/problems/sum-root-to-leaf-numbers/ "求根到叶子节点数字之和 | 力扣（LeetCode）"
[2]: https://leetcode-cn.com/problems/sum-root-to-leaf-numbers/solution/qiu-gen-dao-xie-zi-jie-dian-shu-zi-zhi-he-by-leetc/ "求根到叶子节点数字之和 | 题解（LeetCode）"
