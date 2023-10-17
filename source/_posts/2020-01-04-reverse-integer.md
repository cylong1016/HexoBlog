---
title: 整数反转
date: 2020-01-04 15:37:26
updated: 2020-01-04 15:37:26
categories:
    - LeetCode
tags:
    - LeetCode
    - Java
    - 学习笔记
    - 整数
---
---

# 题目描述

给出一个 32 位的有符号整数，你需要将这个整数中每位上的数字进行反转。

**示例 1:**
> 输入: 123
> 输出: 321

**示例 2:**
> 输入: -123
> 输出: -321

**示例 3:**
> 输入: 120
> 输出: 21

**注意:**
假设我们的环境只能存储得下 32 位的有符号整数，则其数值范围为 [−2³¹,  2³¹ − 1]。请根据这个假设，如果反转后整数溢出那么就返回 0。

<!-- more -->

# 题解

根据题意，我们定义返回值为res，初始为0，我们对入参不断的进行 `x % 10` 操作，这样就会倒序取得每一位数，同时做 `x /= 10` 操作更新x的值，针对res，我们每次需要做 `res * 10 + x % 10` 操作，这样就对x进行了反转。考虑到溢出的情况，我们这边投机取巧，将res定义为long类型，这样int类型的整数就不会溢出。最后判断反转的值是否对int型数溢出即可。

```java
public int reverse(int x) {
    long res = 0;
    while (x != 0) {
        res = res * 10 + x % 10;
        x /= 10;
    }
    if (res > Integer.MAX_VALUE || res < Integer.MIN_VALUE) {
        return 0;
    }
    return (int) res;
}
```

如果res使用int型，那么我们需要在处理 `res = res * 10 + x % 10;`之前判断整数是否溢出。通过判断 `(Integer.MAX_VALUE - x % 10) * 1.0 / 10 >= res`即可，针对负数同理。

## 复杂度分析

* 时间复杂度：O(logn)，对于每次迭代，我们会将输入除以 10，因此时间复杂度为 O(logn)，也可以理解为输入的整数的位数。
* 空间复杂度：Ο(1)。

# 来源

> [整数反转 | 力扣（LeetCode）][1]
> [整数反转 | 题解（LeetCode）][2]

---

[1]: https://leetcode-cn.com/problems/reverse-integer/ "整数反转 | 力扣（LeetCode）"
[2]: https://leetcode-cn.com/problems/reverse-integer/solution/zheng-shu-fan-zhuan-by-leetcode/ "整数反转 | 题解（LeetCode）"
