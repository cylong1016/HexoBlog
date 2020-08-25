---
title: 字符串相加
date: 2020-08-03 23:35:34
updated: 2020-08-03 23:35:34
categories:
    - LeetCode
tags:
    - leetcode
    - java
    - 学习笔记
    - 字符串
---
---

# 题目描述

给定两个字符串形式的非负整数 num1 和 num2 ，计算它们的和。

**提示：**
* num1 和 num2 的长度都小于 5100
* num1 和 num2 都只包含数字 0-9
* num1 和 num2 都不包含任何前导零
* 你不能使用任何內建 BigInteger 库， 也不能直接将输入的字符串转换为整数形式

<!-- more -->

# 题解

我们只需要对两个大整数模拟「竖式加法」的过程。竖式加法就是我们平常学习生活中常用地对两个整数相加的方法，回想一下我们在纸上对两个整数相加的操作，是不是如下图将相同数位对齐，从低到高逐位相加，如果当前位和超过 10，则向高位进一位？因此我们只要将这个过程用代码写出来即可。我们使用两个变量分别指向两个数的末尾，进行加法操作，同时使用 carry 保存进位，若两个字符串不一样长，我们两个字符串遍历完成后，要继续遍历未计算的字符串，最后，别忘了，如果进位是 1 的话，要把进位加到最终的结果中。

{% asset_img 字符串相加.png 字符串相加 %}

```java
public String addStrings(String num1, String num2) {
    int index1 = num1.length() - 1;
    int index2 = num2.length() - 1;
    StringBuilder res = new StringBuilder();
    int carry = 0;
    while (index1 >=0 && index2 >= 0) {
        int n1 = num1.charAt(index1) - '0';
        int n2 = num2.charAt(index2) - '0';
        carry = addNum(n1, n2, carry, res);
        index1--;
        index2--;
    }

    while (index1 >= 0) {
        int n1 = num1.charAt(index1) - '0';
        carry = addNum(n1, 0, carry, res);
        index1--;
    }

    while (index2 >= 0) {
        int n2 = num2.charAt(index2) - '0';
        carry = addNum(0, n2, carry, res);
        index2--;
    }

    if (carry > 0) {
        res.append(carry);
    }
    return res.reverse().toString();
}

public int addNum(int n1, int n2, int carry, StringBuilder res) {
    int sum = n1 + n2 + carry;
    res.append(sum % 10);
    return sum / 10;
}
```

这里，我们其实不需要这么长的代码，判断最后哪个字符串更长，针对短的字符串，我们进行补 0 操作即可。也不需要判断是否有进位，最后还要记得加上去。我们可以简化代码如下。

```java
public String addStrings(String num1, String num2) {
    int index1 = num1.length() - 1;
    int index2 = num2.length() - 1;
    int carry = 0;
    StringBuilder res = new StringBuilder();
    while (index1 >= 0 || index2 >= 0 || carry != 0) {
        int x = index1 >= 0 ? num1.charAt(index1) - '0' : 0;
        int y = index2 >= 0 ? num2.charAt(index2) - '0' : 0;
        int sum = x + y + carry;
        res.append(sum % 10);
        carry = sum / 10;
        index1--;
        index2--;
    }
    return res.reverse().toString();
}
```

## 复杂度分析

* 时间复杂度：Ο(max(m, n))，其中 m 和 n 分别是两个字符串的长度。竖式加法的次数取决于较大数的位数。
* 空间复杂度：O(n)。除答案外我们只需要常数空间存放若干的变量。但是解法中使用到了 StringBuilder，空间复杂度为 O(n)。

# 来源
> [字符串相加 | 力扣（LeetCode）][1]
> [字符串相加 | 题解（LeetCode）][2]

---

> 文章标题：<a href='{{ permalink }}' title='{{ title }}' >{{ title }}</a>
> 文章作者：[cylong](http://www.cylong.com/about/ "cylong")
> 文章链接：<a href='{{ permalink }}' title='{{ title }}' >{{ permalink }}</a>
> 有问题或者建议欢迎在下方评论。欢迎转载、引用，但希望标明出处，感激不尽(●'◡'●)

[1]: https://leetcode-cn.com/problems/add-strings/ "字符串相加 | 力扣（LeetCode）"
[2]: https://leetcode-cn.com/problems/add-strings/solution/zi-fu-chuan-xiang-jia-by-leetcode-solution/ "字符串相加 | 题解（LeetCode）"
