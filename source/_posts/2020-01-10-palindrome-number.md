---
title: 回文数
date: 2020-01-10 16:31:59
updated: 2020-01-10 16:31:59
categories:
    - LeetCode
tags:
    - leetcode
    - java
    - 学习笔记
    - 整数
---
---

# 题目描述

判断一个整数是否是回文数。回文数是指正序（从左向右）和倒序（从右向左）读都是一样的整数。

**示例 1:**
> 输入: 121
> 输出: true

**示例 2:**
> 输入: -121
> 输出: false
> 解释: 从左向右读, 为 -121 。 从右向左读, 为 121- 。因此它不是一个回文数。

**示例 3:**
> 输入: 10
> 输出: false
> 解释: 从右向左读, 为 01 。因此它不是一个回文数。

<!-- more -->

针对输入是负数的情况下，一定不是回文数，因为没有整数是以负号结尾的。接下来考虑，既然回文数是指正序（从左向右）和倒序（从右向左）读都是一样的整数。那么我们将整数反转，判断反转后的数和原来的数是否相等，就可以判断此整数是否是回文数。关于整数反转的操作，可以参考另外一道Leetcode题目：[整数反转][3]

```java
public boolean isPalindrome(int x) {
    if (x < 0) {
        return false;
    }
    int origin = x;
    // 使用long防止整数反转溢出
    long result = 0;
    while (x != 0) {
        result = result * 10 + x % 10;
        x = x / 10;
    }
    // 若反转后的值溢出，则一定不是回文数
    if (result > Integer.MAX_VALUE || result < Integer.MIN_VALUE) {
        return false;
    }
    return (int) result == origin;
}
```

考虑到反转溢出的情况，若整数反转后溢出，那么肯定原来的值和反转后的值不相等，也就是说其实我们上面并不需要特意的去判断反转后的整数是否溢出，于是代码优化如下：

```java
public boolean isPalindrome(int x) {
    if (x < 0) {
        return false;
    }
    int origin = x;
    int result = 0;
    while (x != 0) {
        result = result * 10 + x % 10;
        x = x / 10;
    }
    return result == origin;
}
```

# 复杂度分析

* 时间复杂度：O(logn)，对于每次迭代，我们会将输入除以 10，因此时间复杂度为 O(logn)，也可以理解为输入的整数的位数。
* 空间复杂度：Ο(1)。

# 来源
> [回文数 | 力扣（LeetCode）][1]
> [回文数 | 题解][2]

---

> 文章标题：<a href='{{ permalink }}' title='{{ title }}' >{{ title }}</a>
> 文章作者：[cylong](http://www.cylong.com/about/ "cylong")
> 文章链接：<a href='{{ permalink }}' title='{{ title }}' >{{ permalink }}</a>
> 有问题或者建议欢迎在下方评论。欢迎转载、引用，但希望标明出处，感激不尽(●'◡'●)

[1]: https://leetcode-cn.com/problems/palindrome-number/ "回文数 | 力扣（LeetCode）"
[2]: https://leetcode-cn.com/problems/palindrome-number/solution/hui-wen-shu-by-leetcode-solution/ "回文数 | 题解"
[3]: /blog/2020/01/04/reverse-integer/ "整数反转"
