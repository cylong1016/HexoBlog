---
title: 另一个树的子树
date: 2020-06-22 14:33:13
updated: 2020-06-22 14:33:13
categories:
    - LeetCode
tags:
    - LeetCode
    - Java
    - 学习笔记
    - 递归
    - 树
    - 二叉树
    - 深度优先搜索
---
---

# 题目描述

给定两个非空二叉树 s 和 t，检验 s 中是否包含和 t 具有相同结构和节点值的子树。s 的一个子树包括 s 的一个节点和这个节点的所有子孙。s 也可以看做它自身的一棵子树。

**示例 1:**
```
给定的树 s:
     3
    / \
   4   5
  / \
 1   2

给定的树 t：
   4
  / \
 1   2
返回 true，因为 t 与 s 的一个子树拥有相同的结构和节点值。
```

**示例 2:**
```
给定的树 s：
     3
    / \
   4   5
  / \
 1   2
    /
   0

给定的树 t：
   4
  / \
 1   2
返回 false。
```

<!-- more -->

# 题解

首先我们需要一个可以判断二叉树是否是相同树的方法，使用递归方式处理，递归的结束条件就是 t1 或者 t2 为空，如下：

```java
public boolean isEqual(TreeNode t1, TreeNode t2) {
    if (t1 == null || t2 == null) {
        return t1 == null && t2 == null;
    } else {
        return t1.val == t2.val && isEqual(t1.left, t2.left) && isEqual(t1.right, t2.right);
    }
}
```

接下来，回到原题，判断树 t 是否是树 s 的子树，同样使用递归，不断的判断树 s 的左子树和右子树，是否包含子树 t，递归的结束条件就是树 s 为空，或者树 s 与树 t 相等。

```java
public boolean isSubtree(TreeNode s, TreeNode t) {
    if (s == null) {
        return false;
    }
    if (isEqual(s, t)) {
        return true;
    }
    return isSubtree(s.left, t) || isSubtree(s.right, t);
}
```

# 来源

> [另一个树的子树 | 力扣（LeetCode）][1]
> [另一个树的子树 | 题解（LeetCode）][2]

---

> 文章标题：<a href='{{ permalink }}' title='{{ title }}' >{{ title }}</a>
> 文章作者：[cylong](http://www.cylong.com/about/ "cylong")
> 文章链接：<a href='{{ permalink }}' title='{{ title }}' >{{ permalink }}</a>
> 有问题或者建议欢迎在下方评论。欢迎转载、引用，但希望标明出处，感激不尽(●'◡'●)

[1]: https://leetcode-cn.com/problems/subtree-of-another-tree/ "另一个树的子树 | 力扣（LeetCode）"
[2]: https://leetcode-cn.com/problems/subtree-of-another-tree/solution/ling-yi-ge-shu-de-zi-shu-by-leetcode-solution/ "另一个树的子树 | 题解（LeetCode）"
