---
title: 二叉树的前序遍历
date: 2020-10-27 23:15:09
updated: 2020-10-27 23:15:09
categories:
    - LeetCode
tags:
    - LeetCode
    - Java
    - 学习笔记
    - 树
    - 二叉树
    - 深度优先搜索
    - 递归
---
---

# 题目描述

给你二叉树的根节点 root ，返回它节点值的 前序 遍历。

**示例 1：**
{% asset_img inorder.jpg 前序遍历 %}
> 输入：root = [1, null, 2, 3]
> 输出：[1, 2, 3]

**示例 2：**
> 输入：root = []
> 输出：[]

<!-- more -->

# 递归与非递归版本

前序遍历的输出顺序就是根节点 -> 左子树 -> 右子树。前序遍历是先输出根节点的值，再去递归的输出左子树和右子树。整个遍历过程就是递归的性质，我们可以直接使用递归来完成计算。非递归版本其实是等阶的，只是我们将递归的栈显示的表达出来。下面是递归的版本解法，非递归的解法和二叉树的其他遍历方式可以参考我的另外一篇博客：[二叉树的遍历][3]

```java
List<Integer> ans = new ArrayList<>();

public List<Integer> preorderTraversal(TreeNode root) {
    dfsPreOrder(root);
    return ans;
}

public void dfsPreOrder(TreeNode root) {
    if (root == null) {
        return;
    }
    ans.add(root.val);
    dfsPreOrder(root.left);
    dfsPreOrder(root.right);
}
```

## 复杂度分析

* 时间复杂度：O(n)，其中 n 是二叉树的节点数。每一个节点恰好被遍历一次。
* 空间复杂度：O(n)，为迭代过程中显式栈的开销，平均情况下为 O(logn)，最坏情况下树呈现链状，为 O(n)。

# Morris 遍历

所谓再简单的题，通过看官方的题解，总能发现惊喜。有一种巧妙的方法可以在线性时间内，只占用常数空间来实现前序遍历。这种方法由 J. H. Morris 在 1979 年的论文「Traversing Binary Trees Simply and Cheaply」中首次提出，因此被称为 Morris 遍历。
                        
Morris 遍历的核心思想是利用树的大量空闲指针，实现空间开销的极限缩减。其前序遍历规则总结如下：
1. 新建临时节点，令该节点为 root；
2. 如果当前节点的左子节点为空，将当前节点加入答案，并遍历当前节点的右子节点；
3. 如果当前节点的左子节点不为空，在当前节点的左子树中找到当前节点在中序遍历下的前驱节点：
4. 如果前驱节点的右子节点为空，将前驱节点的右子节点设置为当前节点。然后将当前节点加入答案，并将前驱节点的右子节点更新为当前节点。当前节点更新为当前节点的左子节点。
5. 如果前驱节点的右子节点为当前节点，将它的右子节点重新设为空。当前节点更新为当前节点的右子节点。
6. 重复步骤 2 和步骤 3，直到遍历结束。

```java
public List<Integer> preorderTraversal(TreeNode root) {
    List<Integer> res = new ArrayList<Integer>();
    if (root == null) {
        return res;
    }

    TreeNode p1 = root, p2 = null;
    while (p1 != null) {
        p2 = p1.left;
        if (p2 != null) {
            while (p2.right != null && p2.right != p1) {
                p2 = p2.right;
            }
            if (p2.right == null) {
                res.add(p1.val);
                p2.right = p1;
                p1 = p1.left;
                continue;
            } else {
                p2.right = null;
            }
        } else {
            res.add(p1.val);
        }
        p1 = p1.right;
    }
    return res;
}
```

## 复杂度分析

* 时间复杂度：O(n)，其中 n 是二叉树的节点数。没有左子树的节点只被访问一次，有左子树的节点被访问两次。
* 空间复杂度：O(1)。只操作已经存在的指针（树的空闲指针），因此只需要常数的额外空间。

# 来源

> [二叉树的前序遍历 | 力扣（LeetCode）][1]
> [二叉树的前序遍历 | 题解（LeetCode）][2]

---

> 文章标题：<a href='{{ permalink }}' title='{{ title }}' >{{ title }}</a>
> 文章作者：[cylong](http://www.cylong.com/about/ "cylong")
> 文章链接：<a href='{{ permalink }}' title='{{ title }}' >{{ permalink }}</a>
> 有问题或者建议欢迎在下方评论。欢迎转载、引用，但希望标明出处，感激不尽(●'◡'●)

[1]: https://leetcode-cn.com/problems/binary-tree-preorder-traversal/ "二叉树的前序遍历 | 力扣（LeetCode）"
[2]: https://leetcode-cn.com/problems/binary-tree-preorder-traversal/solution/er-cha-shu-de-qian-xu-bian-li-by-leetcode-solution/ "二叉树的前序遍历 | 题解（LeetCode）"
[3]: /blog/2020/09/02/binary-tree-traverse/ "二叉树的遍历"