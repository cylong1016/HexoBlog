---
title: 一次遍历计算买卖股票的最佳时机（LeetCode 121）
date: 2025-08-21 11:05:21
updated: 2025-08-21 11:05:21
categories:
    - LeetCode
tags:
    - LeetCode
    - LeetCode中等
    - Java
    - 学习笔记
    - 贪心算法
---
---

# 题目描述

给定一个数组 `prices` ，它的第 `i` 个元素 `prices[i]` 表示一支给定股票第 `i` 天的价格。

你只能选择 **某一天** 买入这只股票，并选择在 未来的某一个不同的日子 卖出该股票。设计一个算法来计算你所能获取的最大利润。返回你可以从这笔交易中获取的最大利润。如果你不能获取任何利润，返回 `0` 。

**示例 1:**
> **输入：**`prices = [7, 1, 5, 3, 6, 4]`
> **输出：**`5`
> **解释：**在第 2 天（股票价格 = 1）的时候买入，在第 5 天（股票价格 = 6）的时候卖出，最大利润 = 6-1 = 5 。注意利润不能是 7-1 = 6, 因为卖出价格需要大于买入价格；同时，你不能在买入前卖出股票。

**示例 2:**
> **输入：**`prices = [7, 6, 4, 3, 1]`
> **输出：**`0`
> **解释：**在这种情况下, 没有交易完成, 所以最大利润为 0。

**提示:**
* `1 <= prices.length <= 10^5`
* `0 <= prices[i] <= 10^4`

<!-- more -->

# 贪心算法

## 核心思路

我们可以通过遍历数组一次来计算最大利润。在遍历过程中，记录当前遇到的最小价格，并计算当前价格与最小价格的差值（即利润），同时更新最大利润。
* **初始化：**将最小价格设置为一个很大的值（如 `Integer.MAX_VALUE`），将最大利润设置为 `0`。
* **遍历数组：**
    * 如果当前价格小于最小价格，更新最小价格。
    * 否则，计算当前价格与最小价格的差值，如果大于当前最大利润，则更新最大利润。
* **返回最大利润**

## 代码实现

```java
public int maxProfit(int[] prices) {
    // 如果数组为空或长度为零，直接返回0
    if (prices == null || prices.length == 0) {
        return 0;
    }
    // 初始化最小价格为最大整数，最大利润为0
    int minPrice = Integer.MAX_VALUE;
    int maxProfit = 0;
    // 遍历价格数组
    for (int i = 0; i < prices.length; i++) {
        if (prices[i] < minPrice) {
            // 如果当前价格小于最小价格，更新最小价格
            minPrice = prices[i];
        } else if (prices[i] - minPrice > maxProfit) {
            // 如果当前价格减去最小价格大于最大利润，更新最大利润
            maxProfit = prices[i] - minPrice;
        }
    }
    return maxProfit;
}
```

## 复杂度分析

* **时间复杂度：**`O(n)`，其中 `n` 是数组的长度。我们只需要遍历数组一次。
* **空间复杂度：**`O(1)`，只使用了常数级别的额外空间。

# 来源

> [121. 买卖股票的最佳时机 | 力扣（LeetCode）][1]

---

[1]: https://leetcode.cn/problems/best-time-to-buy-and-sell-stock/description/ "121. 买卖股票的最佳时机 | 力扣（LeetCode）"
