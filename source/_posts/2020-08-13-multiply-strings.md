---
title: 字符串相乘
date: 2020-08-13 02:32:04
updated: 2020-08-13 02:32:04
categories:
    - LeetCode
tags:
    - LeetCode
    - Java
    - 学习笔记
    - 字符串
    - 数学
    - 数组
---
---

# 题目描述

给定两个以字符串形式表示的非负整数 num1 和 num2，返回 num1 和 num2 的乘积，它们的乘积也表示为字符串形式。

**示例 1:**
> 输入: num1 = "2", num2 = "3"
> 输出: "6"

**示例 2:**
> 输入: num1 = "123", num2 = "456"
> 输出: "56088"

**说明：**
1. num1 和 num2 的长度小于 110。
2. num1 和 num2 只包含数字 0-9。
3. num1 和 num2 均不以零开头，除非是数字 0 本身。
4. 不能使用任何标准库的大数类型（比如 BigInteger）或直接将输入转换为整数来处理。

<!-- more -->

# 竖式乘法

第一种方式比较简单，我们只要回想起我们平时计算乘法的方法。如果 num1 和 num2 之一是 0，则直接返回 0 即可。如果 num1 和 num2 都不是 0，则可以通过模拟「竖式乘法」的方法计算乘积。从右往左遍历乘数，将乘数的每一位与被乘数相乘得到对应的结果，再将每次得到的结果累加。需要注意的是，num2 除了最低位以外，其余的每一位的运算结果都需要补 0。

{% asset_img 做加法.png 做加法 %}

关于字符串相加的代码，可以参考：[字符串相加 | 笑话人生][3]

```java
public String multiply(String num1, String num2) {
    if ("0".equals(num1) || "0".equals(num2)) {
        return "0";
    }

    String result = "0";
    for (int i = num2.length() - 1; i >= 0; i--) {
        int n1 = num2.charAt(i) - '0';
        int carry = 0;
        StringBuilder tmpResult = new StringBuilder();
        for (int j = num2.length() - 1; j > i; j--) {
            tmpResult.append(0);
        }
        for (int j = num1.length() - 1; j >= 0; j--) {
            int n2 = num1.charAt(j) - '0';
            int tmp = n1 * n2 + carry;
            carry = tmp / 10;
            tmpResult.append(tmp % 10);
        }
        if (carry > 0) {
            tmpResult.append(carry);
        }
        result = addString(result, tmpResult.reverse().toString());
    }

    return result;
}

public String addString(String num1, String num2) {
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

* 时间复杂度：O(mn + n²)，其中 m 和 n 分别是 num1 和 num2 的长度。需要从右往左遍历 num2，对于 num2 的每一位，都要和 num1 的每一位计算乘积，因此计算乘积的总数是 mn，字符串相加操作共有 n 次，相加的字符串长度最长为 `m + n`，因此字符串相加的时间复杂度是O(mn + n²)。总时间复杂度是O(mn + n²)。
* 空间复杂度：O(m + n)，其中 m 和 n 分别是 num1 和 num2 的长度。空间复杂度取决于存储中间状态的字符串，由于乘积的最大长度为 `m + n`，因此存储中间状态的字符串的长度不会超过 `m + n`。

# 直接做乘积

上一个方法从右往左遍历乘数，将乘数的每一位与被乘数相乘得到对应的结果，再将每次得到的结果累加，整个过程中涉及到较多字符串相加的操作。如果使用数组代替字符串存储结果，则可以减少对字符串的操作。令 m 和 n 分别表示 num1 和 num2 的长度，并且它们均不为 0，则 num1 和 num2 的乘积的长度为 `m + n - 1` 或 `m + n`。

由于 num1 和 num2 的乘积的最大长度为 `m + n`，因此创建长度为 `m + n` 的数组 ansArr 用于存储乘积。对于任意 `0 ≤ i < m` 和 `0 ≤ j < n`，`num1[i] × num2[j]` 的结果位于 `ansArr[i + j + 1]`，如果 `ansArr[i + j + 1] ≥ 10`，则将进位部分加到`ansArr[i + j]`。最后，将数组 ansArr 转成字符串，如果最高位是 0 则舍弃最高位。

```java
public String multiply(String num1, String num2) {
    if ("0".equals(num1) || "0".equals(num2)) {
        return "0";
    }

    int m = num1.length();
    int n = num2.length();
    int[] ansArr = new int[m + n];
    for (int i = m - 1; i >= 0; i--) {
        int x = num1.charAt(i) - '0';
        for (int j = n - 1; j >= 0; j--) {
            int y = num2.charAt(j) - '0';
            ansArr[i + j + 1] += x * y;
        }
    }
    for (int i = m + n - 1; i > 0; i--) {
        ansArr[i - 1] += ansArr[i] / 10;
        ansArr[i] %= 10;
    }
    StringBuilder ans = new StringBuilder();
    for (int i = ansArr[0] == 0 ? 1 : 0; i < m + n; i++) {
        ans.append(ansArr[i]);
    }
    return ans.toString();
}
```

## 复杂度分析

* 时间复杂度：O(mn)，其中 m 和 n 分别是 num1 和 num2 的长度。需要计算 num1 的每一位和 num2 的每一位的乘积。
* 空间复杂度：O(m + n)，其中 m 和 n 分别是 num1 和 num2 的长度。需要创建一个长度为 `m + n` 的数组存储乘积。

# 来源

> [字符串相乘 | 力扣（LeetCode）][1]
> [字符串相乘 | 题解（LeetCode）][2]

---

[1]: https://leetcode-cn.com/problems/multiply-strings/ "字符串相乘 | 力扣（LeetCode）"
[2]: https://leetcode-cn.com/problems/multiply-strings/solution/zi-fu-chuan-xiang-cheng-by-leetcode-solution/ "字符串相乘 | 题解（LeetCode）"
[3]: /blog/2020/08/03/add-strings/ "字符串相加 | 笑话人生"
