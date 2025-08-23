---
title: 魔术索引
date: 2020-08-04 00:37:24
updated: 2020-08-04 00:37:24
categories:
    - LeetCode
tags:
    - LeetCode
    - Java
    - 学习笔记
    - 程序员面试金典
    - 递归
    - 二分查找
    - 数组
    - 剪枝
---
---

# 题目描述

魔术索引。 在数组 A[0...n-1] 中，有所谓的魔术索引，满足条件 A[i] = i 。给定一个有序整数数组，编写一种方法找出魔术索引，若有的话，在数组 A 中找出一个魔术索引，如果没有，则返回 -1。若有多个魔术索引，返回索引值最小的一个。

**示例1:**
> 输入: nums = [0, 2, 3, 4, 5]
> 输出: 0
> 说明: 0 下标的元素为 0

**示例2:**
> 输入: nums = [1, 1, 1]
> 输出: 1

**说明:**
> nums 长度在 [1, 1000000] 之间

<!-- more -->

# 遍历

最简单的方法，我们只要从头遍历整个数组，找到最小的 A[i] = i 返回即可。

```java
public int findMagicIndex(int[] nums) {
    for (int i = 0; i < nums.length; i++) {
        if (nums[i] == i) {
            return i;
        }
    }
    return -1;
}
```

## 复杂度分析

* 时间复杂度：O(n)，其中 n 为数组的长度。
* 空间复杂度：O(1)。

# 二分查找剪枝

本方法我们是进行一定程度的优化，在一些情况下会达到较优的时间复杂度，在最差情况下仍会退化成线性的时间复杂度。现在我们假设，题目中的魔术索引仅有一个，我们假设这个答案为 i，那么意味着 [0...i−1] 的值均小于自身的下标，[i+1...n−1] 的值均大于自身的下标【不理解的同学用反证法即可证明】。那么我们只要使用二分法查找数组中的元素，在 O(log n) 的时间内找到答案 A[i] = i 所在的下标。但是题目中魔术索引可能有多个，我们就需要对二分查找做一些处理。针对二分查找做一下剪枝，策略如下：
* 每次我们选择数组中间的元素，如果当前中间元素左半部分有满足条件的答案，那么这个位置往后的右半边元素我们都不再考虑，只要寻找左半部分满足条件的答案即可。
* 接下来我们继续查看左半部分是否有满足条件的答案，否则如果没有的话我们仍然需要在右半边寻找，使用的策略同上。

显然，此剪枝策略在 [−1, 0, 1, 2, 4] 这种答案为数组的最后一个元素的情况下会退化成线性的时间复杂度，但是在一些情况下会有不错的表现。

```java
public int findMagicIndex(int[] nums) {
    return findMagicIndex(nums, 0, nums.length - 1);
}

public int findMagicIndex(int[] nums, int start, int end) {
    if (start == end) {
        return nums[start] == start ? start : -1;
    }
    int mid = start + ((end - start) >> 1);
    int index = findMagicIndex(nums, start, mid);
    if (index != -1) {
        return index;
    }
    return findMagicIndex(nums, mid + 1, end);
}
```

## 复杂度分析

* 时间复杂度：最坏情况下会达到 O(n) 的时间复杂度，其中 n 为数组的长度。
* 空间复杂度：递归函数的空间取决于调用的栈深度，而最坏情况下我们会递归 n 层，即栈深度为 O(n)，因此空间复杂度最坏情况下为 O(n)。

# 来源

> [魔术索引 | 力扣（LeetCode）][1]
> [魔术索引 | 题解（LeetCode）][2]

---

[1]: https://leetcode-cn.com/problems/magic-index-lcci/ "魔术索引 | 力扣（LeetCode）"
[2]: https://leetcode-cn.com/problems/magic-index-lcci/solution/mo-zhu-suo-yin-by-leetcode-solution/ "魔术索引 | 题解（LeetCode）"
