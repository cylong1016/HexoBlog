---
title: 无重复字符的最长子串
date: 2019-11-26 23:12:36
updated: 2019-11-26 23:12:36
categories:
    - LeetCode
tags:
    - leetcode
    - java
    - 学习笔记
    - 指针
    - 双指针
    - 队列
    - 链表
    - 滑动窗口
---
---

# 题目描述

给定一个字符串，请你找出其中不含有重复字符的最长子串的长度。

**示例 1:**
> 输入: "abcabcbb"
> 输出: 3
> 解释: 因为无重复字符的最长子串是 "abc"，所以其长度为 3。

**示例 2:**
> 输入: "bbbbb"
> 输出: 1
> 解释: 因为无重复字符的最长子串是 "b"，所以其长度为 1。

**示例 3:**
> 输入: "pwwkew"
> 输出: 3
> 解释: 因为无重复字符的最长子串是 "wke"，所以其长度为 3。
> 注意，你的答案必须是子串的长度，"pwke" 是一个子序列，不是子串。

<!-- more -->

# 队列实现

本题是计算最长的不重复子串，而子串肯定是连续的。我们肯定都能想到，要遍历下输入的字符串，那么遍历的过程中，我们需要做什么呢？既然是计算字串的长度，那么我们遍历的过程中就要将字串保存下来。同时，每次保存新的字符的时候，需要判断原有的子串中是否包含了这个字符，如果包含了，那么我们要从字串的第一个字符开始，一直删除字符，直到不存在即将要加入的字符，然后计算当前子串的长度，与之前计算的长度比较，取较大值。拿 `abcdefce` 举例，我们遍历到第二个c字符的时候，已有的不含有重复字符的子串是 `abcdef` ，当要把c加入到已有的子串的时候，需要将前面的  `abc` 删除，那么新的子串为 `defc`。由于子串有后进后出的特性，于是我们使用队列来保存子串。

```java
public int lengthOfLongestSubstring1(String s) {
    int count = 0;
    Queue<Character> queue = new LinkedList<>();
    int length = s.length();
    for (int i = 0; i < length; i++) {
        char c = s.charAt(i);
        while (queue.contains(c)) {
            queue.poll();
        }
        queue.offer(c);
        count = Math.max(queue.size(), count);
    }
    return count;
}
```

# 双指针实现

双指针实现的思路和队列的实现一致，只不过使用两个指针i和j来指向子串的两端，判断 `s.substring(i, j)` 中是否包含当前的字符，若包含，则移动左指针i++，否则移动右指针j++。

```java
public int lengthOfLongestSubstring(String s) {
    if (s.length() <= 1) {
        return s.length();
    }

    int maxLength = 0, i = 0, j = 1;
    while (j < s.length()) {
        if (!s.substring(i, j).contains(String.valueOf(s.charAt(j)))) {
            maxLength = Math.max(maxLength, j - i + 1);
            j++;
        } else {
            if (i < j) {
                i++;
            } else {
                j++;
            }
        }
    }
    return maxLength;
}
```

# 来源
> [无重复字符的最长子串 | 力扣（LeetCode）][1]
> [无重复字符的最长子串 | 题解（LeetCode）][2]

---

> 文章标题：<a href='{{ permalink }}' title='{{ title }}' >{{ title }}</a>
> 文章作者：[cylong](http://www.cylong.com/about/ "cylong")
> 文章链接：<a href='{{ permalink }}' title='{{ title }}' >{{ permalink }}</a>
> 有问题或者建议欢迎在下方评论。欢迎转载、引用，但希望标明出处，感激不尽(●'◡'●)

[1]: https://leetcode-cn.com/problems/longest-substring-without-repeating-characters/ "无重复字符的最长子串 | 力扣（LeetCode）"
[2]: https://leetcode-cn.com/problems/longest-substring-without-repeating-characters/solution/wu-zhong-fu-zi-fu-de-zui-chang-zi-chuan-by-leetc-2/ "无重复字符的最长子串 | 题解（LeetCode）"
