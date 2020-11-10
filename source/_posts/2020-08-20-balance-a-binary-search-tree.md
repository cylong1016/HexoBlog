---
title: 将二叉搜索树变平衡
date: 2020-08-20 16:25:13
updated: 2020-08-20 16:25:13
categories:
    - LeetCode
tags:
    - leetcode
    - java
    - 学习笔记
    - 递归
    - 树
    - 二叉树
    - 二叉搜索树
    - 平衡二叉树
    - 深度优先搜索
    - 中序遍历
    - 贪心算法
---
---

# 题目描述

给你一棵二叉搜索树，请你返回一棵平衡后的二叉搜索树，新生成的树应该与原来的树有着相同的节点值。如果一棵二叉搜索树中，每个节点的两棵子树高度差不超过 1 ，我们就称这棵二叉搜索树是平衡的。如果有多种构造方法，请你返回任意一种。

**示例：**

{% asset_img 二叉搜索树.png 二叉搜索树 %}
{% asset_img 平衡二叉搜索树.png 平衡二叉搜索树 %}

> 输入：root = [1, null, 2, null, 3, null, 4, null, null]
> 输出：[2, 1, 3, null, null, null, 4]
> 解释：这不是唯一的正确答案，[3, 1, 4, null, 2, null, null] 也是一个可行的构造方案。

**提示：**
* 树节点的数目在 1 到 10^4 之间。
* 树节点的值互不相同，且在 1 到 10^5 之间。

<!-- more -->

# 题解

「平衡」要求它是一棵空树或它的左右两个子树的高度差的绝对值不超过 1，这很容易让我们产生这样的想法——左右子树的大小越「平均」，这棵树会不会越平衡？于是一种贪心策略就形成了：我们可以通过中序遍历将原来的二叉搜索树转化为一个有序序列，然后对这个有序序列递归建树，对于区间 [L, R]：

* 取 `mid = (L + R) / 2`，即中心位置做为当前节点的值。
* 如果 `L ≤ mid − 1`，那么递归地将区间 `[L, mid − 1]` 作为当前节点的左子树。
* 如果 `mid + 1 ≤ R`，那么递归地将区间 `[mid + 1, R]` 作为当前节点的右子树。

经过证明此方法是可行的，关于证明方式在此不做赘述，想要了解的同学可以参考下面官方的题解。

```java
List<Integer> treeValList = new ArrayList<>();

public TreeNode balanceBST(TreeNode root) {
    dfsGetTreeValList(root);
    return buildBalanceBST(0, treeValList.size() - 1);
}

private TreeNode buildBalanceBST(int left, int right) {
    int mid = right - ((right - left) >> 1);
    TreeNode node = new TreeNode(treeValList.get(mid));
    node.left = left < mid ? buildBalanceBST(left, mid - 1) : null;
    node.right = mid < right ? buildBalanceBST(mid + 1, right) : null;
    return node;
}

private void dfsGetTreeValList(TreeNode node) {
    if (node == null) {
        return;
    }
    dfsGetTreeValList(node.left);
    treeValList.add(node.val);
    dfsGetTreeValList(node.right);
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

* 时间复杂度：O(n)，获得中序遍历的时间代价是 O(n)；建立平衡二叉树的时建立每个点的时间代价为 O(1)，总时间也是 O(n)。故渐进时间复杂度为 O(n)。
* 空间复杂度：O(n)，这里使用了一个数组作为辅助空间，存放中序遍历后的有序序列，故渐进空间复杂度为 O(n)。

# 来源
> [将二叉搜索树变平衡 | 力扣（LeetCode）][1]
> [将二叉搜索树变平衡 | 题解（LeetCode）][2]

---

> 文章标题：<a href='{{ permalink }}' title='{{ title }}' >{{ title }}</a>
> 文章作者：[cylong](http://www.cylong.com/about/ "cylong")
> 文章链接：<a href='{{ permalink }}' title='{{ title }}' >{{ permalink }}</a>
> 有问题或者建议欢迎在下方评论。欢迎转载、引用，但希望标明出处，感激不尽(●'◡'●)

[1]: https://leetcode-cn.com/problems/balance-a-binary-search-tree/ "将二叉搜索树变平衡 | 力扣（LeetCode）"
[2]: https://leetcode-cn.com/problems/balance-a-binary-search-tree/solution/jiang-er-cha-sou-suo-shu-bian-ping-heng-by-leetcod/ "将二叉搜索树变平衡 | 题解（LeetCode）"
