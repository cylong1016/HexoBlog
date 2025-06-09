---
title: 最大子数组和
date: 2025-06-09 22:40:57
updated: 2025-06-09 22:40:57
categories:
    - LeetCode
tags:
    - LeetCode
    - LeetCode中等
    - Java
    - 学习笔记
    - 数组
    - 子数组
    - 前缀和
    - 动态规划
---
---

# 题目描述

给你一个整数数组 `nums` ，请你找出一个具有最大和的连续子数组（子数组最少包含一个元素），返回其最大和。

**子数组** 是数组中的一个连续部分。

**示例 1:**
> **输入：**`nums = [-2, 1, -3, 4, -1, 2, 1, -5, 4]`
> **输出：**6
> **解释：**连续子数组 `[4, -1, 2, 1]` 的和最大，为 `6` 。

**示例 2:**
> **输入：**`nums = [1]`
> **输出：**1

**示例 3:**
> **输入：**`nums = [5, 4, -1, 7, 8]`
> **输出：**23

**提示:**
* `1 <= nums.length <= 10^5`
* `-10^4 <= nums[i] <= 10^4`

<!-- more -->

# 暴力枚举

最简单的思路，通过两层循环遍历所有可能的连续子数组，计算每个子数组的和并记录最大值。

```java
public int maxSubArray(int[] nums) {
    int max = nums[0];
    for (int i = 0; i < nums.length; i++) {
        int sum = 0;
        for (int j = i; j < nums.length; j++) {
            sum += nums[j];
            max = Math.max(max, sum);
        }
    }
    return max;
}
```

## 复杂度分析

* **时间复杂度：**`O(n²)`，其中 `n` 是数组长度。需要两层循环遍历所有子数组。
* **空间复杂度：**`O(1)`，仅使用常数级别的额外空间。

# 前缀和

参考前缀和的定义：[303. 区域和检索 - 数组不可变 | 笑话人生][3]

利用前缀和的思想，由于子数组的元素和等于两个前缀和的差，遍历数组计算前缀和，同时维护当前最小前缀和。最大子数组和即为当前前缀和与最小前缀和的差值的最大值。

```java
public int maxSubArray(int[] nums) {
    int max = nums[0];
    int prefixSum = 0;
    int minPrefixSum = 0;
    for (int i = 0; i < nums.length; i++) {
        prefixSum += nums[i];
        max = Math.max(max, prefixSum - minPrefixSum);
        minPrefixSum = Math.min(minPrefixSum, prefixSum);
    }
    return max;
}
```

## 复杂度分析

* **时间复杂度：**`O(n)`，只需一次遍历数组。
* **空间复杂度：**`O(1)`，仅使用常数级别的额外空间。

# 动态规划

定义 `dp[i]` 表示以 `nums[i]` 结尾的最大子数组和。状态转移方程为：`dp[i] = Math.max(dp[i - 1], 0) + nums[i]`。最终结果为 `dp` 数组中的最大值。可以简单的理解为，如果 `nums[i]` 左边的子数组和 `dp[i - 1] < 0`，那么加上 `nums[i]` 的值会导致子数组和更小，所以这个时候取 `nums[i]` 的值即可，否则就将 `nums[i]` 的值加入 `dp[i - 1]` 的子数组和。

```java
public int maxSubArray(int[] nums) {
    int[] dp = new int[nums.length];
    dp[0] = nums[0];
    int max = dp[0];
    for (int i = 1; i < nums.length; i++) {
        dp[i] = Math.max(dp[i - 1], 0) + nums[i];
        max = Math.max(max, dp[i]);
    }
    return max;
}
```

## 复杂度分析

* **时间复杂度：**`O(n)`，只需一次遍历数组。
* **空间复杂度：**`O(n)`，需要额外的 `dp` 数组存储中间结果。

# 动态规划（空间优化）

在以上动态规划的基础上进行空间优化。由于 `dp[i]` 只依赖于 `dp[i - 1]`，因此可以用一个变量代替 `dp` 数组。

```java
public int maxSubArray(int[] nums) {
    int dp = nums[0];
    int max = nums[0];
    for (int i = 1; i < nums.length; i++) {
        dp = Math.max(dp, 0) + nums[i];
        max = Math.max(max, dp);
    }
    return max;
}
```

## 复杂度分析

* **时间复杂度：**`O(n)`，只需一次遍历数组。
* **空间复杂度：**`O(1)`，仅使用常数级别的额外空间。

# 总结

| 解法      | 时间复杂度 | 空间复杂度 | 特点 |
| ----------- | ----------- | ----------- | ----------- |
| 暴力枚举	      | O(n²)       | O(1)       | 思路简单，效率低       |
| 前缀和   | O(n)        | O(1)        | 利用前缀和思想        |
| 动态规划   | O(n)        | O(n)        | 标准动态规划解法        |
| 动态规划优化   | O(n)        | O(1)        | 最优解法，推荐使用        |

# 来源

> [53. 最大子数组和 | 力扣（LeetCode）][1]
> [53. 最大子数组和 | 题解 | 灵茶山艾府][2]

---

[1]: https://leetcode.cn/problems/maximum-subarray/description/ "53. 最大子数组和 | 力扣（LeetCode）"
[2]: https://leetcode.cn/problems/maximum-subarray/solutions/2533977/qian-zhui-he-zuo-fa-ben-zhi-shi-mai-mai-abu71/?envType=study-plan-v2&envId=top-100-liked "53. 最大子数组和 | 题解 | 灵茶山艾府"
[3]: /blog/2025/06/03/range-sum-query-immutable/ "303. 区域和检索 - 数组不可变 | 笑话人生"
