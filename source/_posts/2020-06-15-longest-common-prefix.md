---
title: 最长公共前缀
date: 2020-06-15 14:47:42
updated: 2020-06-15 14:47:42
categories:
    - LeetCode
tags:
    - leetcode
    - java
    - 学习笔记
    - 字符串
    - 数组
    - 递归
    - 分治法
    - 指针
    - 双指针
---
---

# 题目描述

编写一个函数来查找字符串数组中的最长公共前缀。如果不存在公共前缀，返回空字符串 ""。

**示例 1:**

> 输入: ["flower", "flow", "flight"]
> 输出: "fl"

**示例 2:**
> 输入: ["dog", "racecar", "car"]
> 输出: ""
> 解释: 输入不存在公共前缀。

**说明:**
> 所有输入只包含小写字母 a-z 。

<!-- more -->

# 横向扫描

我们求所有字符串的最长公共前缀，可以发现，对于字符串S₁……Sn，我们只要依次遍历每个字符串，对于每个遍历到的字符串，更新当前的最长公共前缀，遍历所有的字符串以后，即可得到字符串数组中的最长公共前缀。下面是给了递归和遍历两种方式代码。如果在尚未遍历完所有的字符串时，最长公共前缀已经是空串，则最长公共前缀一定是空串，因此不需要继续遍历剩下的字符串，直接返回空串即可。

```java
public String longestCommonPrefix(String[] strs) {
    if (strs == null || strs.length == 0) {
        return "";
    } else if (strs.length == 1) {
        return strs[0];
    } else {
        return longestCommonPrefixTwoStr(strs[0], longestCommonPrefix(strs, 1));
    }
}

public String longestCommonPrefix(String[] strs, int index) {
    if (index == strs.length - 1) {
        return strs[index];
    }
    // 递归处理当前字符串和(后续所有字符传最长公共前缀)的最长公共前缀。
    return longestCommonPrefixTwoStr(strs[index], longestCommonPrefix(strs, index + 1));
}

public String longestCommonPrefixTwoStr(String str1, String str2) {
    int len = Math.min(str1.length(), str2.length());
    int index = 0;
    while (index < len && str1.charAt(index) == str2.charAt(index)) {
        index++;
    }
    return str1.substring(0, index);
}
```

```java
public String longestCommonPrefix(String[] strs) {
    if (strs == null || strs.length == 0) {
        return "";
    }
    String prefix = strs[0];
    for (int i = 1; i < strs.length; i++) {
        prefix = longestCommonPrefixTwoStr(prefix, strs[i]);
        if (prefix.length() == 0) {
            break;
        }
    }
    return prefix;
}

public String longestCommonPrefixTwoStr(String str1, String str2) {
    int len = Math.min(str1.length(), str2.length());
    int index = 0;
    while (index < len && str1.charAt(index) == str2.charAt(index)) {
        index++;
    }
    return str1.substring(0, index);
}
```

## 复杂度分析

* 时间复杂度：O(mn)，其中 m 是字符串数组中的字符串的平均长度，n 是字符串的数量。最坏情况下，字符串数组中的每个字符串的每个字符都会被比较一次。
* 空间复杂度：遍历是O(1)，只需要额外的常数级别的空间。递归是O(n)，其中 n 是字符串的数量。

# 纵向扫描

前面的方法是横向扫描，依次遍历每个字符串，更新最长公共前缀。另一种方法是纵向扫描。纵向扫描时，从前往后遍历所有字符串的每一列，比较相同列上的字符是否相同，如果相同则继续对下一列进行比较，如果不相同则当前列不再属于公共前缀，当前列之前的部分为最长公共前缀。

![纵向扫描](纵向扫描.png)

```java
public String longestCommonPrefix(String[] strs) {
    if (strs == null || strs.length == 0) {
        return "";
    }
    int length = strs[0].length();
    for (int i = 0; i < length; i++) {
        char c = strs[0].charAt(i);
        for (int j = 1; j < strs.length; j++) {
            if (i == strs[j].length() || strs[j].charAt(i) != c) {
                return strs[0].substring(0, i);
            }
        }
    }
    return strs[0];
}
```

## 复杂度分析

* 时间复杂度：O(mn)，其中 m 是字符串数组中的字符串的平均长度，n 是字符串的数量。最坏情况下，字符串数组中的每个字符串的每个字符都会被比较一次。
* 空间复杂度：O(1)，只需要额外的常数级别的空间。

# 分治法

针对横向扫描的方法，我们可以发现，可以使用分治法得到字符串的最长公共前缀，我们可以将字符串数组一分为二，分别求分开的两个字符串数组的最长公共前缀，并不断递归处理，最后合并求出子问题的最长公共前缀。

![分治法](分治法.png)

```java
public String longestCommonPrefix(String[] strs) {
    if (strs == null || strs.length == 0) {
        return "";
    } else {
        return longestCommonPrefix(strs, 0, strs.length - 1);
    }
}

public String longestCommonPrefix(String[] strs, int start, int end) {
    if (start == end) {
        return strs[start];
    } else {
        int mid = (end - start) / 2 + start;
        String left = longestCommonPrefix(strs, start, mid);
        String right = longestCommonPrefix(strs, mid + 1, end);
        return longestCommonPrefixTwoStr(left, right);
    }
}

public String longestCommonPrefixTwoStr(String str1, String str2) {
    int len = Math.min(str1.length(), str2.length());
    int index = 0;
    while (index < len && str1.charAt(index) == str2.charAt(index)) {
        index++;
    }
    return str1.substring(0, index);
}
```

## 复杂度分析

* 时间复杂度：O(mn)，其中 m 是字符串数组中的字符串的平均长度，n 是字符串的数量。时间复杂度的递推式是 T(n)=2⋅T(n/2)+O(m)，通过计算可得 T(n)=O(mn)。
* 空间复杂度：O(mlogn)，其中 m 是字符串数组中的字符串的平均长度，n 是字符串的数量。空间复杂度主要取决于递归调用的层数，层数最大为 logn，每层需要 m 的空间存储返回结果。

# 来源
> [最长公共前缀 | 力扣（LeetCode）][1]
> [最长公共前缀 | 题解（LeetCode）][2]

---

> 文章标题：<a href='{{ permalink }}' title='{{ title }}' >{{ title }}</a>
> 文章作者：[cylong](http://www.cylong.com/about/ "cylong")
> 文章链接：<a href='{{ permalink }}' title='{{ title }}' >{{ permalink }}</a>
> 有问题或者建议欢迎在下方评论。欢迎转载、引用，但希望标明出处，感激不尽(●'◡'●)

[1]: https://leetcode-cn.com/problems/longest-common-prefix/ "最长公共前缀 | 力扣（LeetCode）"
[2]: https://leetcode-cn.com/problems/longest-common-prefix/solution/zui-chang-gong-gong-qian-zhui-by-leetcode-solution/ "最长公共前缀 | 题解（LeetCode）"
