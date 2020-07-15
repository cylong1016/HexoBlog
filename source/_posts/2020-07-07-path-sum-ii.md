---
title: 路径总和 II
date: 2020-07-07 00:03:49
updated: 2020-07-07 00:03:49
categories:
    - LeetCode
tags:
    - leetcode
    - java
    - 学习笔记
    - 递归
    - 树
    - 二叉树
    - 深度优先搜索
    - 回溯算法
    - 中序遍历
---
---

# 题目描述

给定一个二叉树和一个目标和，找到所有从根节点到叶子节点路径总和等于给定目标和的路径。
**说明: 叶子节点是指没有子节点的节点。**

**示例:**
给定如下二叉树，以及目标和 sum = 22，
```
      5
     / \
    4   8
   /   / \
  11  13  4
 /  \    / \
7    2  5   1
```
返回:
```
[
   [5,4,11,2],
   [5,8,4,5]
]
```

<!-- more -->

# 题解

做此题之前，可以先做一道简单版的[路径总和][3]，简单版的只要遍历全部节点，遇到满足条件的路径返回 true 即可，而此题不仅要遍历全部节点，还要记录满足条件的路径。此题是典型的回溯算法，思路就是，先声明一个`LinkedList<Integer> path`保存路径，`List<List<Integer>> ans`保存所有路径，我们每遍历到一个节点，就将这个节点的值保存在 path 中，当判断到子节点，如果此节点的值满足 sum 等于 val，则将 path 加入到 ans 中，否则继续进行递归遍历左右子树。注意入参 root 可能为空，另外递归的结束条件也包含节点为空。当递归回溯的时候，我们就删除最后加入到 path 中的节点。

```java
List<List<Integer>> ans = new LinkedList<>();
LinkedList<Integer> path = new LinkedList<>();

public List<List<Integer>> pathSum(TreeNode root, int sum) {
    if (root == null) {
        return ans;
    }
    path.add(root.val);
    if (root.left == null && root.right == null) {
        if (sum == root.val) {
            // 此处new LinkedList<>(path)新建一个链表
            // 因为如果直接add(path)，ans中的路径和path是相同的引用，后面操作path后，ans中的路径也将一起被修改。
            ans.add(new LinkedList<>(path));
            // 此处无需return，因为后续pathSum递归的时候root.left和root.right都为空，满足递归结束条件。
        }
    }
    pathSum(root.left, sum - root.val);
    pathSum(root.right, sum - root.val);
    // 递归回溯的时候删除最后一个节点，此处使用LinkedList提升效率
    path.removeLast();
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

# 来源
> [路径总和 II | 力扣（LeetCode）][1]
> [路径总和 II | 题解][2]

---

> 文章标题：<a href='{{ permalink }}' title='{{ title }}' >{{ title }}</a>
> 文章作者：[cylong](http://www.cylong.com/about/ "cylong")
> 文章链接：<a href='{{ permalink }}' title='{{ title }}' >{{ permalink }}</a>
> 有问题或者建议欢迎在下方评论。欢迎转载、引用，但希望标明出处，感激不尽(●'◡'●)

[1]: https://leetcode-cn.com/problems/path-sum-ii/ "路径总和 II | 力扣（LeetCode）"
[2]: https://leetcode-cn.com/problems/path-sum-ii/solution/ "路径总和 II | 题解"
[3]: https://leetcode-cn.com/problems/path-sum/ "路径总和 | 力扣（LeetCode）"
