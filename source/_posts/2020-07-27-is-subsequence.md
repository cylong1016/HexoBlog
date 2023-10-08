---
title: 判断子序列
date: 2020-07-27 16:39:31
updated: 2020-07-27 16:39:31
categories:
    - LeetCode
tags:
    - LeetCode
    - Java
    - 学习笔记
    - 字符串
    - 指针
    - 双指针
---
---

# 题目描述

给定字符串 s 和 t ，判断 s 是否为 t 的子序列。你可以认为 s 和 t 中仅包含英文小写字母。字符串 t 可能会很长（长度 ≈ 500,000），而 s 是个短字符串（长度 <= 100）。

字符串的一个子序列是原始字符串删除一些（也可以不删除）字符而不改变剩余字符相对位置形成的新字符串。（例如，"ace"是"abcde"的一个子序列，而"aec"不是）。

**示例 1:**
> s = "abc", t = "ahbgdc"
> 返回 true.

**示例 2:**
> s = "axc", t = "ahbgdc"
> 返回 false.

<!-- more -->

# 双指针

我们使用双指针i和j分别指向短的字符串和长的字符串，移动规则是，当`s[i] == t[j]`的时候，同时移动i和j，否则，我们不断的移动j指针，直到找到`s[i] == t[j]`，然后同时移动i和j，直到遍历完字符串s。最后，如果s是t的子串，那么j一定小于t.length。

```java
public boolean isSubsequence(String s, String t) {
    char[] sArr = s.toCharArray();
    char[] tArr = t.toCharArray();
    int i = 0, j = 0;
    for (; i < sArr.length; i++, j++) {
        while (j < tArr.length && sArr[i] != tArr[j]) {
            j++;
        }
    }
    // 题目中在for循环中同时操作i++和j++，所以此处是小于等于的关系
    return j <= tArr.length;
}
```

## 复杂度分析

* 时间复杂度：O(n + m)，其中 n 为 s 的长度，m 为 t 的长度。每次无论是匹配成功还是失败，都有至少一个指针发生右移，两指针能够位移的总距离为 n + m。
* 空间复杂度：O(1)。

# 来源

> [判断子序列 | 力扣（LeetCode）][1]
> [判断子序列 | 题解（LeetCode）][2]

---

> 文章标题：<a href='{{ permalink }}' title='{{ title }}' >{{ title }}</a>
> 文章作者：[cylong](http://www.cylong.com/about/ "cylong")
> 文章链接：<a href='{{ permalink }}' title='{{ title }}' >{{ permalink }}</a>
> 有问题或者建议欢迎在下方评论。欢迎转载、引用，但希望标明出处，感激不尽(●'◡'●)

[1]: https://leetcode-cn.com/problems/is-subsequence/ "判断子序列 | 力扣（LeetCode）"
[2]: https://leetcode-cn.com/problems/is-subsequence/solution/pan-duan-zi-xu-lie-by-leetcode-solution/ "判断子序列 | 题解（LeetCode）"
