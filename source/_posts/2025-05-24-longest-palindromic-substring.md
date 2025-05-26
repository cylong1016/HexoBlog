---
title: 最长回文子串
date: 2025-05-24 12:02:04
updated: 2025-05-24 12:02:04
categories:
    - LeetCode
tags:
    - LeetCode
    - LeetCode中等
    - Java
    - 学习笔记
    - 动态规划
---
---

# 题目描述

给你一个字符串 s，找到 s 中最长的回文子串。

**说明:**
* 如果字符串向前和向后读都相同，则它满足回文性。
* 子字符串是字符串中连续的非空字符序列。

**示例 1:**
> 输入：s = "babad"
> 输出："bab"
> 解释："aba" 同样是符合题意的答案。

**示例 2:**
> 输入：s = "cbbd"
> 输出："bb"

**提示:**
* 1 <= s.length <= 1000
* s 仅由数字和英文字母组成

<!-- more -->

# 暴力法（不推荐）

枚举所有子串，检查是否为回文，记录最长的一个。

```java
public String longestPalindrome(String s) {
    if (s == null || s.isEmpty()) {
        return "";
    }
    int start = 0;
    int maxLen = 0;
    for (int i = 0; i < s.length(); i++) {
        for (int j = i; j < s.length(); j++) {
            if (longestPalindrome(s, i, j) && (j - i + 1 > maxLen)) {
                maxLen = j - i + 1;
                start = i;
            }
        }
    }
    return s.substring(start, start + maxLen);
}

public boolean longestPalindrome(String s, int start, int end) {
    while (start < end) {
        if (s.charAt(start++) != s.charAt(end--)) {
            return false;
        }
    }
    return true;
}
```

## 复杂度分析

* 时间复杂度：O(n³)，共有 O(n²) 个子串（两层循环：i 从 0 到 n-1，j 从 i 到 n-1）。每个子串检查是否为回文需要 O(n) 时间（如长度为 k 的子串需比较 k/2 次）。
总时间 = O(n²) × O(n) = O(n³)。
* 空间复杂度：O(n)，仅需常数空间存储临时变量（如 maxLen, start）。

# 动态规划

利用二维数组 dp[i][j] 记录子串 s[i...j] 是否为回文，逐步扩展长度。

```java
public String longestPalindrome(String s) {
    if (s == null || s.length() <= 1) {
        return s;
    }
    int len = s.length();
    boolean[][] dp = new boolean[len][len];
    int start = 0;
    int maxLen = 1;
    // 长度为1的子串全都是回文串
    for (int i = 0; i < len; i++) {
        dp[i][i] = true;
    }

    // 判断长度为2的子串是否是回文串
    for (int i = 0; i < len - 1; i++) {
        if (s.charAt(i) == s.charAt(i + 1)) {
            dp[i][i + 1] = true;
            start = i;
            maxLen = 2;
        }
    }

    // 判断长度为3及更长的子串是否是回文串
    for (int l = 3; l <= len; l++) {
        for (int i = 0; i < len - l + 1; i++) {
            int j = i + l - 1;
            if (s.charAt(i) == s.charAt(j) && dp[i + 1][j - 1]) {
                dp[i][j] = true;
                start = i;
                maxLen = l;
            }
        }
    }

    return s.substring(start, start + maxLen);
}
```

## 复杂度分析

* 时间复杂度：O(n²)，外层循环遍历子串长度 l（从 3 到 n），内层循环遍历起始点 i，总时间 = O(n²)。
* 空间复杂度：O(n²)，需一个二维数组 dp[n][n] 存储所有子串的状态。

# 中心扩展法

利用二维数组 dp[i][j] 记录子串 s[i...j] 是否为回文，逐步扩展长度。

```java
public String longestPalindrome1(String s) {
    if (s == null || s.isEmpty()) {
        return "";
    }
    int start = 0;
    int end = 0;
    for (int i = 0; i < s.length(); i++) {
        // 奇数长度
        int len1 = expand(s, i, i);
        // 偶数长度
        int len2 = expand(s, i, i + 1);
        int len = Math.max(len1, len2);
        if (len > end - start) {
            start = i - (len - 1) / 2;
            end = i + len / 2;
        }
    }
    return s.substring(start, end + 1);
}

private int expand(String s, int left, int right) {
    while (left >= 0 && right < s.length() && s.charAt(left) == s.charAt(right)) {
        left--;
        right++;
    }
    return right - left - 1;
}
```

## 复杂度分析

* 时间复杂度：O(n²)，共有 2n-1 个中心（每个字符作为奇中心，每两个字符之间作为偶中心），每个中心最多扩展 O(n) 次（如全相同字符时，扩展到边界）。
总时间 = O(2n) × O(n) = O(n²)。
* 空间复杂度：O(1)，仅需常数空间记录扩展的左右指针和最长回文的起止点。

# 来源

> [5. 最长回文子串 | 力扣（LeetCode）][1]

---

[1]: https://leetcode-cn.com/problems/sum-root-to-leaf-numbers/ "5. 最长回文子串 | 力扣（LeetCode）"
