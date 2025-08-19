---
title: 递归操作解析嵌套编码字符串：数字与字符分段处理（LeetCode 394）
date: 2025-08-17 20:50:34
updated: 2025-08-17 20:50:34
categories:
    - LeetCode
tags:
    - LeetCode
    - LeetCode中等
    - Java
    - 学习笔记
    - 栈
    - 递归
---
---

# 题目描述

给定一个经过编码的字符串，返回它解码后的字符串。编码规则为: `k[encoded_string]`，表示其中方括号内部的 `encoded_string` 正好重复 `k` 次。注意 `k` 保证为正整数。

你可以认为输入字符串总是有效的；输入字符串中没有额外的空格，且输入的方括号总是符合格式要求的。此外，你可以认为原始数据不包含数字，所有的数字只表示重复的次数 `k` ，例如不会出现像 `3a` 或 `2[4]` 的输入。

测试用例保证输出的长度不会超过 `10^5`。

**示例 1:**
> **输入：**`s = "3[a]2[bc]"`
> **输出：**`"aaabcbc"`

**示例 2:**
> **输入：**`s = "3[a2[c]]"`
> **输出：**`"accaccacc"`

**示例 3:**
> **输入：**`s = "2[abc]3[cd]ef"`
> **输出：**`"abcabccdcdcdef"`

**示例 4:**
> **输入：**`s = "abc3[cd]xyz"`
> **输出：**`"abccdcdcdxyz"`

**提示:**
* `1 <= s.length <= 30`
* `s` 由小写英文字母、数字和方括号 `'[]'` 组成
* `s` 保证是一个有效的输入。
* `s` 中所有整数的取值范围为 `[1, 300] `

<!-- more -->

# 递归

## 核心思路

使用**递归（DFS）** 逐层解析嵌套括号：
1. 全局指针 `i`：记录当前处理的字符位置。
2. 递归函数 `decode()`：
    * 遇到 **字母**：直接拼接到结果。
    * 遇到 **数字**：计算重复次数 `k`。
    * 遇到 **左括号** `[`：递归解析子字符串，并将结果重复 `k` 次拼接到当前结果。
    * 遇到 **右括号** `]`：结束当前递归层，返回结果。
3. 重置 `k`：每次处理完 `k[encoded_string]` 后重置 `k`，避免影响后续数字。

## 代码实现

```java
// 全局指针
private int i = 0;

public String decodeString(String s) {
    i = 0; // 重置指针（避免多次调用干扰）
    return decode(s.toCharArray());
}

private String decode(char[] s) {
    StringBuilder res = new StringBuilder();
    int k = 0; // 当前重复次数
    
    while (i < s.length) {
        char c = s[i];
        i++;
        if (Character.isLetter(c)) {
            res.append(c); // 字母直接拼接
        } else if (Character.isDigit(c)) {
            k = k * 10 + (c - '0'); // 计算重复次数（处理多位数）
        } else if (c == '[') {
            String sub = decode(s); // 递归解析子字符串
            for (int j = 0; j < k; j++) {
                res.append(sub); // 重复拼接子字符串
            }
            k = 0; // 重置重复次数
        } else if (c == ']') {
            break; // 结束当前递归层
        }
    }
    return res.toString();
}
```

## 复杂度分析

* **时间复杂度：**`O(n)`，其中 `n` 是解码后字符串的长度。每个字符只处理一次，但重复拼接操作会增加时间。
* **空间复杂度：**`O(m)`，`m` 是递归栈的最大深度（即嵌套括号层数）。

# 来源

> [394. 字符串解码 | 力扣（LeetCode）][1]
> [394. 字符串解码 | 题解 | 灵茶山艾府][2]
> [394. 字符串解码 | 题解 | Krahets][3]

---

[1]: https://leetcode.cn/problems/decode-string/description/ "394. 字符串解码 | 力扣（LeetCode）"
[2]: https://leetcode.cn/problems/decode-string/solutions/3744428/di-gui-yong-zhan-mo-ni-di-gui-pythonjava-kcsv/ "394. 字符串解码 | 题解 | 灵茶山艾府"
[3]: https://leetcode.cn/problems/decode-string/solutions/19447/decode-string-fu-zhu-zhan-fa-di-gui-fa-by-jyd/ "394. 字符串解码 | 题解 | Krahets"
