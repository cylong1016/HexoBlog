---
title: 拥有最多糖果的孩子
date: 2020-06-01 22:33:45
updated: 2020-06-01 22:33:45
categories:
    - LeetCode
tags:
    - LeetCode
    - Java
    - 学习笔记
    - 数组
    - 链表
    - 流API
---
---

# 题目描述

给你一个数组 candies 和一个整数 extraCandies ，其中 candies[i] 代表第 i 个孩子拥有的糖果数目。对每一个孩子，检查是否存在一种方案，将额外的 extraCandies 个糖果分配给孩子们之后，此孩子有最 的糖果。
**注意：** 允许有多个孩子同时拥有最多的糖果数目。

**示例 1：**
> 输入：candies = [2, 3, 5, 1, 3], extraCandies = 3
> 输出：[true, true, true, false, true]
> 解释：
> 孩子 1 有 2 个糖果，如果他得到所有额外的糖果（3个），那么他总共有 5 个糖果，他将成为拥有最多糖果的孩子。
> 孩子 2 有 3 个糖果，如果他得到至少 2 个额外糖果，那么他将成为拥有最多糖果的孩子。
> 孩子 3 有 5 个糖果，他已经是拥有最多糖果的孩子。
> 孩子 4 有 1 个糖果，即使他得到所有额外的糖果，他也只有 4 个糖果，无法成为拥有糖果最多的孩子。
> 孩子 5 有 3 个糖果，如果他得到至少 2 个额外糖果，那么他将成为拥有最多糖果的孩子。

**示例 2：**
> 输入：candies = [4, 2, 1, 1, 2], extraCandies = 1
> 输出：[true, false, false, false, false]
> 解释：只有 1 个额外糖果，所以不管额外糖果给谁，只有孩子 1 可以成为拥有糖果最多的孩子。

**示例 3：**
> 输入：candies = [12, 1, 12], extraCandies = 10
> 输出：[true, false, true]

**提示：**
* 2 <= candies.length <= 100
* 1 <= candies[i] <= 100
* 1 <= extraCandies <= 50

<!-- more -->

# 题解

我们只要先遍历一遍小朋友拥有的糖果数，找出最大值，然后再遍历一次小朋友的糖果数，加上额外的糖果，是否大于等于之前求出的最大值即可。

```java
public List<Boolean> kidsWithCandies(int[] candies, int extraCandies) {
    OptionalInt max = Arrays.stream(candies).max();
    List<Boolean> result = new LinkedList<>();
    for (int num : candies) {
        result.add(num + extraCandies >= max.getAsInt());
    }
    return result;
}
```

此题使用了Java流API `Arrays.stream(candies).max()`，可以很方便的处理集合或者数组相关的问题，比如数组求和，求最大值等等。关于流API的介绍可以参考：
> [Java 8 的 Lambda 表达式和 Stream API][3]

另外此题是六一儿童节LeetCode的每日一题，官方是想让我们过一个快乐的六一啊，在这里也祝各位大孩子们六一儿童节快乐\^o^/

## 复杂度分析

* 时间复杂度：O(n)，我们首先使用 O(n) 的时间预处理出所有小朋友拥有的糖果数目最大值。对于每一个小朋友，我们需要 O(1) 的时间判断这个小朋友是否可以拥有最多的糖果，故渐进时间复杂度为 O(n)。
* 空间复杂度：Ο(1)。

# 来源

> [拥有最多糖果的孩子 | 力扣（LeetCode）][1]
> [拥有最多糖果的孩子 | 题解（LeetCode）][2]

---

[1]: https://leetcode-cn.com/problems/kids-with-the-greatest-number-of-candies/ "拥有最多糖果的孩子 | 力扣（LeetCode）"
[2]: https://leetcode-cn.com/problems/kids-with-the-greatest-number-of-candies/solution/yong-you-zui-duo-tang-guo-de-hai-zi-by-leetcode-so/ "拥有最多糖果的孩子 | 题解（LeetCode）"
[3]: /blog/2019/03/18/lambda/ "Java 8 的 Lambda 表达式和 Stream API"
