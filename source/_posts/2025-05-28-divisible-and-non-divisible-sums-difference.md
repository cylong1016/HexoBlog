---
title: 分类求和并作差
date: 2025-05-28 00:19:46
updated: 2025-05-28 00:19:46
categories:
    - LeetCode
tags:
    - LeetCode
    - LeetCode简单
    - Java
    - 学习笔记
    - 数学公式
---
---

# 题目描述

给你两个正整数 `n` 和 `m` 。现定义两个整数 `num1` 和 `num2` ，如下所示：

`num1`：范围 `[1, n]` 内所有 无法被 `m` 整除 的整数之和。
`num2`：范围 `[1, n]` 内所有 能够被 `m` 整除 的整数之和。

返回整数 `num1 - num2` 。

**示例 1:**
> **输入：**n = 10, m = 3
> **输出：**19
> **解释：**在这个示例中：
> 范围 `[1, 10]` 内无法被 3 整除的整数为 `[1, 2, 4, 5, 7, 8, 10]` ，num1 = 这些整数之和 = 37 。
> 范围 `[1, 10]` 内能够被 3 整除的整数为 `[3, 6, 9]` ，num2 = 这些整数之和 = 18 。
> 返回 37 - 18 = 19 作为答案。

**示例 2:**
> **输入：**n = 5, m = 6
> **输出：**15
> **解释：**在这个示例中：
> 范围 `[1, 5]` 内无法被 6 整除的整数为 `[1, 2, 3, 4, 5]` ，num1 = 这些整数之和 =  15 。
> 范围 `[1, 5]` 内能够被 6 整除的整数为 `[]` ，num2 = 这些整数之和 = 0 。
> 返回 15 - 0 = 15 作为答案。

**示例 3:**
> **输入：**n = 5, m = 1
> **输出：**-15
> **解释：**在这个示例中：
> 范围 `[1, 5]` 内无法被 1 整除的整数为 `[]` ，num1 = 这些整数之和 = 0 。 
> 范围 `[1, 5]` 内能够被 1 整除的整数为 `[1, 2, 3, 4, 5]` ，num2 = 这些整数之和 = 15 。
> 返回 0 - 15 = -15 作为答案。

**提示:**
* `1 <= n, m <= 1000`

<!-- more -->

# 普通遍历

最直观方法遍历 `1` 到 `n` 的每个数，分别累加能被 `m` 整除的数的和 `sum_div` 和不能整除的数的和 `sum_non_div` ，最后返回 `sum_non_div - sum_div`。

```java
public int differenceOfSums(int n, int m) {
    int sum_div = 0;
    int sum_non_div = 0;
    for (int i = 1; i <= n; i++) {
        if (i % m == 0) {
            sum_div += i;
        } else {
            sum_non_div += i;
        }
    }

    return sum_non_div - sum_div;
}
```

## 复杂度分析

* **时间复杂度：**`O(n)`，需遍历 `1` 到 `n` 的所有数。
* **空间复杂度：**`O(1)`，仅使用常数空间。

# 数学公式

利用数学公式直接计算，避免遍历：

1. 总和：`1` 到 `n` 的和为 `total = n * (n + 1) / 2`。
2. 整除和：能被 `m` 整除的数形如 `m, 2m, 3m, ..., km`（其中 `k = n / m`），和为 `m * (k * (k + 1) / 2)`。
3. 差值：非整除和 - 整除和 = `(total - sum_div) - sum_div = total - 2 * sum_div`。

```java
public int differenceOfSums(int n, int m) {
    // 计算 1 到 n 的总和
    long total = (long) n * (n + 1) / 2;
    // 计算可整除数的个数k
    int k = n / m;
    // 可整除数的和
    long divisibleSum = (long) m * k * (k + 1) / 2;
    // 返回差值
    return (int) (total - 2 * divisibleSum);
}
```

## 复杂度分析

* **时间复杂度：**`O(1)`。
* **空间复杂度：**`O(1)`。


# 来源

> [2894. 分类求和并作差 | 力扣（LeetCode）][1]

---

[1]: https://leetcode.cn/problems/divisible-and-non-divisible-sums-difference/description/ "2894. 分类求和并作差 | 力扣（LeetCode）"