---
title: 最长重复子数组
date: 2020-07-01 00:46:55
updated: 2020-07-01 00:46:55
categories:
    - LeetCode
tags:
    - leetcode
    - java
    - 学习笔记
    - 动态规划
    - 滑动窗口
    - 数组
---
---

# 题目描述

给两个整数数组 A 和 B ，返回两个数组中公共的、长度最长的子数组的长度。

**示例：**
> 输入：
> A: [1, 2, 3, 2, 1]
> B: [3, 2, 1, 4, 7]
> 输出：3
> 解释：长度最长的公共子数组是 [3, 2, 1] 。

**提示：**
* 1 <= len(A), len(B) <= 1000
* 0 <= A[i], B[i] < 100

<!-- more -->

# 暴力方法

最简单的方法，暴力方法，我们知道最长公共子数组长度最大值为数组A和B中长度较小值。我们先固定最长值为min(len(A), len(B))，然后遍历A和B，判断此长度是否有公共子数组，找到，则返回此值。

```java
public int findLength(int[] A, int[] B) {
    int len = Math.min(A.length, B.length);
    int res_len = len;
    while (res_len > 0) {
        for (int ai = 0; ai + res_len <= len; ai++) {
            for (int bi = 0; bi + res_len <= len; bi++) {
                // 判断固定长度的子数组是否相等
                if (subArrayIsSame(A, B, ai, bi, res_len)) {
                    return res_len;
                }
            }
        }
        // 如果不相等，最大长度减一
        res_len--;
    }
    return res_len;
}

public boolean subArrayIsSame(int[] A, int[] B, int ai, int bi, int len) {
    for (int i = 0; i < len; i++) {
        if (A[ai + i] != B[bi + i]) {
            return false;
        }
    }
    return true;
}
```

## 复杂度分析

* 时间复杂度：Ο(N³)，因为要遍历A、B和公共子数组。
* 空间复杂度：Ο(1)。

# 滑动窗口

以题目为例，它们的最长重复子数组是 [3, 2, 1]，在 A 与 B 中的开始位置不同。但如果我们知道了开始位置，我们就可以根据它们将 A 和 B 进行「对齐」，即：

```
A = [1, 2, 3, 2, 1]
B =       [3, 2, 1, 4, 7]
           ↑  ↑  ↑
```

此时，最长重复子数组在 A 和 B 中的开始位置相同，我们就可以对这两个数组进行一次遍历，得到子数组的长度。我们可以枚举 A 和 B 所有的对齐方式。对齐的方式有两类：第一类为 A 不变，B 的首元素与 A 中的某个元素对齐；第二类为 B 不变，A 的首元素与 B 中的某个元素对齐。对于每一种对齐方式，我们计算它们相对位置相同的重复子数组即可。

{% asset_img 最长重复子数组.gif 最长重复子数组 %}

```java
public int findLength(int[] A, int[] B) {
    int lenA = A.length;
    int lenB = B.length;
    int ret = 0;
    for (int i = 0; i < lenA; i++) {
        int len = Math.min(lenA - i, lenB);
        int maxLen = maxLength(A, B, i, 0, len);
        ret = Math.max(ret, maxLen);
    }

    for (int i = 0; i < lenB; i++) {
        int len = Math.min(lenB - i, lenA);
        int maxLen = maxLength(A, B, 0, i, len);
        ret = Math.max(ret, maxLen);
    }
    return ret;
}

// 对齐后计算最长公共子数组
public int maxLength(int[] A, int[] B, int indexA, int indexB, int len) {
    int ret = 0;
    int maxLen = 0;
    for (int i = 0; i < len; i++) {
        if (A[indexA + i] == B[indexB + i]) {
            maxLen++;
        } else {
            maxLen = 0;
        }
        ret = Math.max(ret, maxLen);
    }
    return ret;
}
```

## 复杂度分析

* 时间复杂度： Ο((M + N) × min(M, N))。
* 空间复杂度： Ο(1)。
> M 表示数组 A 的长度，N 表示数组 B 的长度。

# 动态规划

我们使用A[i]，B[j]分别表示两个数组对应下标的值。如果 A[i] == B[j]，那么我们知道 A[i:] 与 B[j:] 的最长公共前缀为 A[i + 1:] 与 B[j + 1:] 的最长公共前缀的长度加一，否则我们知道 A[i:] 与 B[j:] 的最长公共前缀为零。这样我们就可以提出动态规划的解法：令 dp[i][j] 表示 A[i:] 和 B[j:] 的最长公共前缀，那么答案即为所有 dp[i][j] 中的最大值。如果 A[i] == B[j]，那么 dp[i][j] = dp[i + 1][j + 1] + 1，否则 dp[i][j] = 0。
考虑到这里 dp[i][j] 的值从 dp[i + 1][j + 1] 转移得到，所以我们需要倒过来，首先计算 dp[len(A) - 1][len(B) - 1]，最后计算 dp[0][0]。

```java
public int findLength(int[] A, int[] B) {
    int n = A.length, m = B.length;
    int[][] dp = new int[n + 1][m + 1];
    int ret = 0;
    for (int i = n - 1; i >= 0; i--) {
        for (int j = m - 1; j >= 0; j--) {
            dp[i][j] = A[i] == B[j] ? dp[i + 1][j + 1] + 1 : 0;
            ret = Math.max(ret, dp[i][j]);
        }
    }
    return ret;
}
```

## 复杂度分析

* 时间复杂度： O(M × N)。
* 空间复杂度： O(M × N)。
> N 表示数组 A 的长度，M 表示数组 B 的长度。

# 来源
> [最长重复子数组 | 力扣（LeetCode）][1]
> [最长重复子数组 | 题解（LeetCode）][2]

---

> 文章标题：<a href='{{ permalink }}' title='{{ title }}' >{{ title }}</a>
> 文章作者：[cylong](http://www.cylong.com/about/ "cylong")
> 文章链接：<a href='{{ permalink }}' title='{{ title }}' >{{ permalink }}</a>
> 有问题或者建议欢迎在下方评论。欢迎转载、引用，但希望标明出处，感激不尽(●'◡'●)

[1]: https://leetcode-cn.com/problems/maximum-length-of-repeated-subarray/ "最长重复子数组 | 力扣（LeetCode）"
[2]: https://leetcode-cn.com/problems/maximum-length-of-repeated-subarray/solution/zui-chang-zhong-fu-zi-shu-zu-by-leetcode-solution/ "最长重复子数组 | 题解（LeetCode）"
