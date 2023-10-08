---
title: 有序数组的平方
date: 2020-10-16 00:11:00
updated: 2020-10-16 00:11:00
categories:
    - LeetCode
tags:
    - LeetCode
    - Java
    - 学习笔记
    - 指针
    - 双指针
    - 数组
---
---

# 题目描述

给定一个按非递减顺序排序的整数数组 A，返回每个数字的平方组成的新数组，要求也按非递减顺序排序。

**示例 1：**
> 输入：[-4, -1, 0, 3, 10]
> 输出：[0, 1, 9, 16, 100]

**示例 2：**
> 输入：[-7, -3, 2, 3, 11]
> 输出：[4, 9, 9, 49, 121]

<!-- more -->

# 双指针

最简单的方法，我们可以将数组中的元素全部求平方，然后进行排序即可，但是这样操作空间复杂度和时间复杂度都较大，在此我们不多做赘述。我们观察数组的特性可以发现，数组是排序好的，这样我们就可以使用一个比较巧妙的方法进行计算，具体的，对于全正数的数组，直接平方后即满足题意，但是有负数的情况下，负数中越小的负数，计算的结果越大。正数中越大的正数计算的结果越大，题目要求平方后的数组依然是非递减顺序排序，于是我们可以定义两个指针分别指向 0 和 len - 1。不断的移动这两个指针，每次我们将平方后的较大的值逆序的放入数组中。最后完成计算，结果也将是非递减顺序。

```java
public int[] sortedSquares(int[] A) {
    int len = A.length;
    int[] ans = new int[len];
    for (int i = 0, j = len - 1, pos = len - 1; i <= j;) {
        if (A[i] * A[i] > A[j] * A[j]) {
            ans[pos] = A[i] * A[i];
            i++;
        } else {
            ans[pos] = A[j] * A[j];
            j--;
        }
        pos--;
    }
    return ans;
}
```

## 复杂度分析

* 时间复杂度：O(n)，其中 n 是数组 A 的长度。
* 空间复杂度：O(1)。

# 来源

> [有序数组的平方 | 力扣（LeetCode）][1]
> [有序数组的平方 | 题解（LeetCode）][2]

---

> 文章标题：<a href='{{ permalink }}' title='{{ title }}' >{{ title }}</a>
> 文章作者：[cylong](http://www.cylong.com/about/ "cylong")
> 文章链接：<a href='{{ permalink }}' title='{{ title }}' >{{ permalink }}</a>
> 有问题或者建议欢迎在下方评论。欢迎转载、引用，但希望标明出处，感激不尽(●'◡'●)

[1]: https://leetcode-cn.com/problems/squares-of-a-sorted-array/ "有序数组的平方 | 力扣（LeetCode）"
[2]: https://leetcode-cn.com/problems/squares-of-a-sorted-array/solution/you-xu-shu-zu-de-ping-fang-by-leetcode-solution/ "有序数组的平方 | 题解（LeetCode）"
