---
title: 验证回文串
date: 2020-06-19 17:43:28
updated: 2020-06-19 17:43:28
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

给定一个字符串，验证它是否是回文串，只考虑字母和数字字符，可以忽略字母的大小写。
**说明：** 本题中，我们将空字符串定义为有效的回文串。

**示例 1:**
> 输入: "A man, a plan, a canal: Panama"
> 输出: true

**示例 2:**
> 输入: "race a car"
> 输出: false

<!-- more -->

# 题解

我们先假设，字符串中仅包含英文字母，那么判断是否是回文串，我们只需要使用两个指针i和j，同时指向字符串的首尾，然后判断i和j指向的字母是否相等，然后同时进行 `i++` 和 `j--` 操作，直到 `i == j`。用这个思路解决此题，由于字符串中包含很多非英文字母，那么我们就需要多一步处理，如果i和j指向的字符不是英文字母，那么我们就不断的进行 `i++` 和 `j--` 操作，直到i和j指向的字符是英文字母，然后进行比较即可。

```java
public boolean isPalindrome(String s) {
    int i = 0;
    int j = s.length() - 1;
    while (i < j) {
        while (i < j && !Character.isLetterOrDigit(s.charAt(i))) {
            i++;
        }
        while (i < j && !Character.isLetterOrDigit(s.charAt(j))) {
            j--;
        }
        if (i < j && Character.toLowerCase(s.charAt(i)) != Character.toLowerCase(s.charAt(j))) {
            return false;
        }
        i++;
        j--;
    }
    return true;
}
```

## 复杂度分析

* 时间复杂度：O(n)，其中 n 是字符串的长度。
* 空间复杂度：O(1)，只需要额外的常数级别的空间。

# 来源

> [验证回文串 | 力扣（LeetCode）][1]
> [验证回文串 | 题解（LeetCode）][2]

---

> 文章标题：<a href='{{ permalink }}' title='{{ title }}' >{{ title }}</a>
> 文章作者：[cylong](http://www.cylong.com/about/ "cylong")
> 文章链接：<a href='{{ permalink }}' title='{{ title }}' >{{ permalink }}</a>
> 有问题或者建议欢迎在下方评论。欢迎转载、引用，但希望标明出处，感激不尽(●'◡'●)

[1]: https://leetcode-cn.com/problems/valid-palindrome/ "验证回文串 | 力扣（LeetCode）"
[2]: https://leetcode-cn.com/problems/valid-palindrome/solution/yan-zheng-hui-wen-chuan-by-leetcode-solution/ "验证回文串 | 题解（LeetCode）"
