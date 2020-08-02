---
title: 跳水板
date: 2020-07-08 23:36:21
updated: 2020-07-08 23:36:21
categories:
    - LeetCode
tags:
    - leetcode
    - java
    - 学习笔记
    - 数学
    - 数组
---
---

# 题目描述

你正在使用一堆木板建造跳水板。有两种类型的木板，其中长度较短的木板长度为shorter，长度较长的木板长度为longer。你必须正好使用k块木板。编写一个方法，生成跳水板所有可能的长度。返回的长度需要从小到大排列。

**示例 1：**
> 输入：
> shorter = 1
> longer = 2
> k = 3
> 输出： [3,4,5,6]
> 解释：可以使用 3 次 shorter，得到结果 3；使用 2 次 shorter 和 1 次 longer，得到结果 4 。以此类推，得到最终结果。

**提示：**
> 0 < shorter <= longer
> 0 <= k <= 100000

<!-- more -->

# 题解

考虑两种情况，如果k为0，那么不会建造任何跳水板，直接返回空数组。另外一种情况，如果shorter与longer相等，那么跳水板的长度是唯一的，就是 `shorter * k` ，于是返回长度为 1 的数组，数组中的元素为 `shorter ∗ k`。

当shorter不等于longer的情况，我们考虑跳水板最短的情况，就是全是短木板，此时的长度是 `shorter * k`，我们每次增加一块长木板，减少一块短木板，由于共有 k 块木板，于是共有 `k + 1` 种长度 `k + 1` 种组合。每次木板的长度变化是 `longer - shorter`。

```java
public int[] divingBoard(int shorter, int longer, int k) {
    if (k == 0) {
        return new int[0];
    }
    int min = shorter * k;
    int difference = longer - shorter;
    if (difference == 0) {
        return new int[]{min};
    }

    int[] ans = new int[k + 1];
    ans[0] = min;
    for (int i = 1; i <= k; i++) {
        ans[i] = ans[i - 1] + difference;
    }
    return ans;
}
```

# 复杂度分析

* 时间复杂度：O(k)，其中 k 是木板数量。短木板和长木板一共使用 k 块，一共有 k + 1 种组合，对于每种组合都要计算跳水板的长度。
* 空间复杂度：O(1)，只需要额外的常数级别的空间。

# 来源
> [跳水板 | 力扣（LeetCode）][1]
> [跳水板 | 题解（LeetCode）][2]

---

> 文章标题：<a href='{{ permalink }}' title='{{ title }}' >{{ title }}</a>
> 文章作者：[cylong](http://www.cylong.com/about/ "cylong")
> 文章链接：<a href='{{ permalink }}' title='{{ title }}' >{{ permalink }}</a>
> 有问题或者建议欢迎在下方评论。欢迎转载、引用，但希望标明出处，感激不尽(●'◡'●)

[1]: https://leetcode-cn.com/problems/diving-board-lcci/ "跳水板 | 力扣（LeetCode）"
[2]: https://leetcode-cn.com/problems/diving-board-lcci/solution/tiao-shui-ban-by-leetcode-solution/ "跳水板 | 题解（LeetCode）"
