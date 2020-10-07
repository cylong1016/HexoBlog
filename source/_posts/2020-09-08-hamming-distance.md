---
title: 汉明距离
date: 2020-09-08 00:10:52
updated: 2020-09-08 00:10:52
categories:
    - LeetCode
tags:
    - leetcode
    - java
    - 学习笔记
    - 位运算
    - 数学
    - Brian Kernighan 算法
---
---

# 题目描述

两个整数之间的汉明距离指的是这两个数字对应二进制位不同的位置的数目。给出两个整数 x 和 y，计算它们之间的汉明距离。

**示例:**
> 输入: x = 1, y = 4
> 输出: 2
> 解释:
```
 1   (0 0 0 1)
 4   (0 1 0 0)
        ↑   ↑
```

<!-- more -->

# 位移运算

根据题意，我们直接使用异或运算两个整数，结果是相同位为 1，不同位为 0，这样我们直接计算异或后整数的 1 的位数，就是汉明距离。检查某一位是否是 1， 可以使用取模运算(i % 2)或者 AND 与运算(i & 1)。

```java
public int hammingDistance(int x, int y) {
    int xor = x ^ y;
    int ans = 0;
    while (xor != 0) {
        if (xor % 2 == 1) {
            ans += 1;
        }
        xor = xor >> 1;
    }
    return ans;
}
```

## 复杂度分析

* 时间复杂度：O(1)，在 Java 中 Integer 的大小是固定的，处理时间也是固定的。 32 位整数需要 32 次迭代。
* 空间复杂度：O(1)，使用恒定大小的空间。

# Brian Kernighan 算法

上面例子中，遇到最右边的 1 后，如果可以跳过中间的 0，直接跳到下一个 1，效率会高很多。这是布赖恩·克尼根位计数算法的基本思想。该算法使用特定比特位和算术运算移除等于 1 的最右比特位。

当我们在 number 和 number - 1 上做 AND 位运算时，原数字 number 的最右边等于 1 的比特会被移除。
{% asset_img BrianKernighan算法.png Brian Kernighan 算法 %}

```java
public int hammingDistance(int x, int y) {
    int xor = x ^ y;
    int ans = 0;
    while (xor != 0) {
        ans += 1;
        xor = xor & (xor - 1);
    }
    return ans;
}
```

## 复杂度分析

* 时间复杂度：O(1)，在 Java 中 Integer 的大小是固定的，处理时间也是固定的。 但是该方法需要的迭代操作更少。
* 空间复杂度：O(1)，使用恒定大小的空间。

# 来源
> [汉明距离 | 力扣（LeetCode）][1]
> [汉明距离 | 题解（LeetCode）][2]

---

> 文章标题：<a href='{{ permalink }}' title='{{ title }}' >{{ title }}</a>
> 文章作者：[cylong](http://www.cylong.com/about/ "cylong")
> 文章链接：<a href='{{ permalink }}' title='{{ title }}' >{{ permalink }}</a>
> 有问题或者建议欢迎在下方评论。欢迎转载、引用，但希望标明出处，感激不尽(●'◡'●)

[1]: https://leetcode-cn.com/problems/hamming-distance/ "汉明距离 | 力扣（LeetCode）"
[2]: https://leetcode-cn.com/problems/hamming-distance/solution/yi-ming-ju-chi-by-leetcode/ "汉明距离 | 题解（LeetCode）"
