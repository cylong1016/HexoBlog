---
title: 验证二叉搜索树
date: 2020-07-15 23:04:31
updated: 2020-07-15 23:04:31
categories:
    - LeetCode
tags:
    - leetcode
    - java
    - 学习笔记
    - 递归
    - 中序遍历
    - 树
    - 二叉树
    - 二叉搜索树
    - 深度优先搜索
---
---

# 题目描述
给定一个二叉树，判断其是否是一个有效的二叉搜索树。假设一个二叉搜索树具有如下特征：

* 节点的左子树只包含小于当前节点的数。
* 节点的右子树只包含大于当前节点的数。
* 所有左子树和右子树自身必须也是二叉搜索树。

**示例 1:**
```
输入:
    2
   / \
  1   3
输出: true
```

**示例 2:**
```
输入:
    5
   / \
  1   4
     / \
    3   6
输出: false
解释: 输入为: [5, 1, 4, null, null, 3, 6]。根节点的值为 5 ，但是其右子节点值为 4 。
```

<!-- more -->

# 递归

由题目给出的信息我们可以知道：如果该二叉树的左子树不为空，则左子树上所有节点的值均小于它的根节点的值；若它的右子树不空，则右子树上所有节点的值均大于它的根节点的值；它的左右子树也为二叉搜索树。

于是我们设计一个递归函数 `isValidBST(root, lower, upper)` 来递归判断，函数表示考虑以 root 为根的子树，判断子树中所有节点的值是否都在 (l, r) 的范围内（注意是开区间）。如果 root 节点的值 val 不在 ((l, r) 的范围内说明不满足条件直接返回，否则我们要继续递归调用检查它的左右子树是否满足，如果都满足才说明这是一棵二叉搜索树。

那么根据二叉搜索树的性质，在递归调用左子树时，我们需要把上界 upper 改为 root.val，即调用 `isValidBST(root.left, lower, root.val)`，因为左子树里所有节点的值均小于它的根节点的值。同理递归调用右子树时，我们需要把下界 lower 改为 root.val，即调用 `isValidBST(root.right, root.val, upper)`。函数递归调用的入口为 `isValidBST(root, -inf, +inf)`， inf 表示一个无穷大的值。

```java
public boolean isValidBST(TreeNode root) {
    // 无穷大和无穷小用null表示，这里不能用Integer.MAX_VALUE和Integer.MIN_VALUE
    return isValidBST(root, null, null);
}

public boolean isValidBST(TreeNode node, Integer lower, Integer upper) {
    if (node == null) {
        return true;
    }

    int val = node.val;
    if (lower != null && val <= lower) {
        return false;
    }
    if (upper != null && val >= upper) {
        return false;
    }

    if (!isValidBST(node.right, val, upper)) {
        return false;
    }
    if (!isValidBST(node.left, lower, val)) {
        return false;
    }
    return true;
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

* 时间复杂度：O(n)，其中 n 为二叉树的节点个数。在递归调用的时候二叉树的每个节点最多被访问一次，因此时间复杂度为 O(n)。
* 空间复杂度：O(n)，其中 n 为二叉树的节点个数。递归函数在递归过程中需要为每一层递归函数分配栈空间，所以这里需要额外的空间且该空间取决于递归的深度，即二叉树的高度。最坏情况下二叉树为一条链，树的高度为 n ，递归最深达到 n 层，故最坏情况下空间复杂度为 O(n) 。

# 中序遍历

我们知道二叉搜索树「中序遍历」得到的值构成的序列一定是升序的，中序遍历是二叉树的一种遍历方式，它先遍历左子树，再遍历根节点，最后遍历右子树。而我们二叉搜索树保证了左子树的节点的值均小于根节点的值，根节点的值均小于右子树的值，因此中序遍历以后得到的序列一定是升序序列。这启示我们在中序遍历的时候实时检查当前节点的值是否大于前一个中序遍历到的节点的值即可。如果均大于说明这个序列是升序的，整棵树是二叉搜索树，否则不是。

```java
long pre = Long.MIN_VALUE;

public boolean isValidBST(TreeNode root) {
    if (root == null) {
        return true;
    }
    // 访问左子树
    if (!isValidBST(root.left)) {
        return false;
    }
    // 访问当前节点：如果当前节点小于等于中序遍历的前一个节点，说明不满足BST，返回 false；否则继续遍历。
    if (root.val <= pre) {
        return false;
    }
    pre = root.val;
    // 访问右子树
    return isValidBST(root.right);
}

public static class TreeNode {
    int val;
    TreeNode left;
    TreeNode right;

    TreeNode(int x) {
        val = x;
    }
}
```

## 复杂度分析

* 时间复杂度：O(n)，其中 n 为二叉树的节点个数。在递归调用的时候二叉树的每个节点最多被访问一次，因此时间复杂度为 O(n)。
* 空间复杂度：O(n)，其中 n 为二叉树的节点个数。递归函数在递归过程中需要为每一层递归函数分配栈空间，所以这里需要额外的空间且该空间取决于递归的深度，即二叉树的高度。最坏情况下二叉树为一条链，树的高度为 n ，递归最深达到 n 层，故最坏情况下空间复杂度为 O(n) 。

# 来源
> [验证二叉搜索树 | 力扣（LeetCode）][1]
> [验证二叉搜索树 | 题解（LeetCode）][2]

---

> 文章标题：<a href='{{ permalink }}' title='{{ title }}' >{{ title }}</a>
> 文章作者：[cylong](http://www.cylong.com/about/ "cylong")
> 文章链接：<a href='{{ permalink }}' title='{{ title }}' >{{ permalink }}</a>
> 有问题或者建议欢迎在下方评论。欢迎转载、引用，但希望标明出处，感激不尽(●'◡'●)

[1]: https://leetcode-cn.com/problems/validate-binary-search-tree/ "验证二叉搜索树 | 力扣（LeetCode）"
[2]: https://leetcode-cn.com/problems/validate-binary-search-tree/solution/yan-zheng-er-cha-sou-suo-shu-by-leetcode-solution/ "验证二叉搜索树 | 题解（LeetCode）"
