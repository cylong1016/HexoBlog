---
title: 组合
date: 2020-09-17 00:51:25
updated: 2020-09-17 00:51:25
categories:
    - LeetCode
tags:
    - leetcode
    - java
    - 学习笔记
    - 递归
    - 深度优先搜索
    - 回溯算法
    - 组合
---
---

# 题目描述

给定两个整数 n 和 k，返回 1 ... n 中所有可能的 k 个数的组合。

**示例:**
> 输入: n = 4, k = 2
> 输出:
```
[
  [2,4],
  [3,4],
  [2,3],
  [1,2],
  [1,3],
  [1,4],
]
```

<!-- more -->

# 递归

从 n 个当中选 k 个的所有方案对应的枚举是组合型枚举。思路很简单，针对 1 ... n 中的每个数，在组合的结果中，我们都有两种结果，选择或者不选择。于是我们从第一个数开始进行递归的判断。详细分析在代码注释中。

```java
List<List<Integer>> ans = new ArrayList<>();
LinkedList<Integer> item = new LinkedList<>();

public List<List<Integer>> combine(int n, int k) {
    dfsCombine(n, k, 1);
    return ans;
}

public void dfsCombine(int n, int k, int index) {
    if (item.size() == k) {
        // 如果长度达到k，保存结果。
        ans.add(new ArrayList<>(item));
        return;
    }

    if (item.size() + n - index + 1 < k) {
        // 如果剩下的数字不够组合成k个数，则不满足要求。
        return;
    }
    // 选择当前元素，然后进行递归。 
    item.add(index);
    dfsCombine(n, k, index + 1);
    
    // 不选择当前元素，然后进行递归，也是一种回溯。
    item.removeLast();
    dfsCombine(n, k, index + 1);
}
```

# 来源

> [组合 | 力扣（LeetCode）][1]
> [组合 | 题解（LeetCode）][2]

---

> 文章标题：<a href='{{ permalink }}' title='{{ title }}' >{{ title }}</a>
> 文章作者：[cylong](http://www.cylong.com/about/ "cylong")
> 文章链接：<a href='{{ permalink }}' title='{{ title }}' >{{ permalink }}</a>
> 有问题或者建议欢迎在下方评论。欢迎转载、引用，但希望标明出处，感激不尽(●'◡'●)

[1]: https://leetcode-cn.com/problems/combinations/ "组合 | 力扣（LeetCode）"
[2]: https://leetcode-cn.com/problems/combinations/solution/zu-he-by-leetcode-solution/ "组合 | 题解（LeetCode）"
