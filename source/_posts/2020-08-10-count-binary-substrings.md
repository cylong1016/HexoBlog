---
title: 计数二进制子串
date: 2020-08-10 23:48:04
updated: 2020-08-10 23:48:04
categories:
    - LeetCode
tags:
    - LeetCode
    - Java
    - 学习笔记
    - 字符串
    - 双指针
---
---

# 题目描述

给定一个字符串 s，计算具有相同数量0和1的非空(连续)子字符串的数量，并且这些子字符串中的所有0和所有1都是组合在一起的。重复出现的子串要计算它们出现的次数。

**示例 1:**
> 输入: "00110011"
> 输出: 6
> 解释: 有 6 个子串具有相同数量的连续1和0：“0011”，“01”，“1100”，“10”，“0011” 和 “01”。
> 请注意，一些重复出现的子串要计算它们出现的次数。
> 另外，“00110011”不是有效的子串，因为所有的0（和1）没有组合在一起。

**示例 2:**
> 输入: "10101"
> 输出: 4
> 解释: 有4个子串：“10”，“01”，“10”，“01”，它们具有相同数量的连续1和0。

**注意：**
> s.length 在1到50,000之间。
> s 只包含“0”或“1”字符。

<!-- more -->

# 题解

刚开始看到，可能没什么思路，但是我们仔细阅读题目，可以慢慢找到规律，首先针对一个满足题意的子串，假如是“000111”，那么满足题意的子串就同时有三种，“000111”和“0011”和“01”。可以发现，满足条件的子串，是以01为中心，左边不断补0，右边不断补1而成，每补一次，就多一个满足条件的子串。针对以10为中心同理。所以我们只要找到位置 i 的字符串和位置 i + 1 的字符串不等，并以此为中心左右扩展，左边的字符等于位置 i 的字符，右边的字符等于位置 i + 1 的字符。扩展一次，满足条件的子串次数就加一。遍历完全部字符后，就能得到答案。

```java
public int countBinarySubstrings(String s) {
    char[] sArr = s.toCharArray();
    int ans = 0;
    for (int i = 0; i < sArr.length - 1; i++) {
        if (sArr[i] != sArr[i + 1]) {
            int m = i;
            int n = i + 1;
            while (m >= 0 && n < sArr.length && sArr[m--] == sArr[i] && sArr[n++] == sArr[i + 1]) {
                ans++;
            }
        }
    }
    return ans;
}
```

## 复杂度分析

* 时间复杂度：O(n)。
* 空间复杂度：O(n)。这里使用 `s.toCharArray()` 仅仅是为了效率，其实可以使用 s.charAt()，这样空间复杂度就是 O(1) 了。

# 来源

> [计数二进制子串 | 力扣（LeetCode）][1]
> [计数二进制子串 | 题解（LeetCode）][2]

---

[1]: https://leetcode-cn.com/problems/count-binary-substrings/ "计数二进制子串 | 力扣（LeetCode）"
[2]: https://leetcode-cn.com/problems/count-binary-substrings/solution/ji-shu-er-jin-zhi-zi-chuan-by-leetcode-solution/ "计数二进制子串 | 题解（LeetCode）"
