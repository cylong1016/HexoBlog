---
title: 划分字母区间
date: 2020-10-22 23:08:50
updated: 2020-10-22 23:08:50
categories:
    - LeetCode
tags:
    - leetcode
    - java
    - 学习笔记
    - 贪心算法
    - 指针
    - 双指针
    - 字符串
    - 递归
---
---

# 题目描述

字符串 S 由小写字母组成。我们要把这个字符串划分为尽可能多的片段，同一字母最多出现在一个片段中。返回一个表示每个字符串片段的长度的列表。

**示例：**
> 输入：S = "ababcbacadefegdehijhklij"
> 输出：[9, 7, 8]
> 解释：
> 划分结果为 "ababcbaca", "defegde", "hijhklij"。
> 每个字母最多出现在一个片段中。像 "ababcbacadefegde", "hijhklij" 的划分是错误的，因为划分的片段数较少。

**提示：**
* S的长度在 [1, 500] 之间。
* S只包含小写字母 'a' 到 'z'。

<!-- more -->

# 贪心算法

由于同一个字母只能出现在同一个片段，显然同一个字母的第一次出现的下标位置和最后一次出现的下标位置必须出现在同一个片段。我们从第一个字母开始遍历，初始的时候，我们认为划分的字符串就是当前字母，即 `maxLen = 1`，然后我们求当前字母的最后一次出现的下标 index。并更新当前划分的字符串最长长度为 `maxLen = Math.max(index + 1, maxLen)`。遍历的截止条件就是 `i < maxLen`。说明已经满足了题目条件。接下来，我们只要递归的处理剩下的字符串即可。

```java
List<Integer> ans = new ArrayList<>();

public List<Integer> partitionLabels(String S) {
    if (S == null || S.length() == 0) {
        return ans;
    }
    int maxLen = 1;
    for (int i = 0; i < maxLen; i++) {
        int index = S.lastIndexOf(S.charAt(i));
        maxLen = Math.max(index + 1, maxLen);
    }

    ans.add(maxLen);
    partitionLabels(S.substring(maxLen));
    return ans;
}
```

参考官方的解法后发现，其实以上代码有几处可以优化的点，首先就是，我们可以先遍历一遍字符串，求出每个字符最后一次出现的下标位置。在得到每个字母最后一次出现的下标位置之后，可以使用贪心算法和双指针的方法将字符串划分为尽可能多的片段，具体做法如下。
                                                  
* 从左到右遍历字符串，遍历的同时维护当前片段的开始下标 start 和结束下标 end，初始时 `start = end = 0`。
* 对于每个访问到的字母 c，得到当前字母的最后一次出现的下标位置 end_c，则当前片段的结束下标一定不会小于 end_c，因此令 `end = max(end, end_c)`。
* 当访问到下标 end 时，当前片段访问结束，当前片段的下标范围是 [start, end]，长度为 `end − start + 1`，将当前片段的长度添加到返回值，然后令 `start = end + 1`，继续寻找下一个片段。
* 重复上述过程，直到遍历完字符串。

```java
public List<Integer> partitionLabels(String S) {
    int[] last = new int[26];
    int length = S.length();
    for (int i = 0; i < length; i++) {
        last[S.charAt(i) - 'a'] = i;
    }
    List<Integer> ans = new ArrayList<Integer>();
    int start = 0;
    int end = 0;
    for (int i = 0; i < length; i++) {
        end = Math.max(end, last[S.charAt(i) - 'a']);
        if (i == end) {
            ans.add(end - start + 1);
            start = end + 1;
        }
    }
    return ans;
}
```

## 复杂度分析
* 时间复杂度：O(n)，其中 n 是字符串的长度。需要遍历字符串两次，第一次遍历时记录每个字母最后一次出现的下标位置，第二次遍历时进行字符串的划分。
* 空间复杂度：O(Σ)，其中 Σ 是字符串中的字符集大小。这道题中，字符串只包含小写字母，因此 Σ = 26。

# 来源

> [划分字母区间 | 力扣（LeetCode）][1]
> [划分字母区间 | 题解（LeetCode）][2]

---

> 文章标题：<a href='{{ permalink }}' title='{{ title }}' >{{ title }}</a>
> 文章作者：[cylong](http://www.cylong.com/about/ "cylong")
> 文章链接：<a href='{{ permalink }}' title='{{ title }}' >{{ permalink }}</a>
> 有问题或者建议欢迎在下方评论。欢迎转载、引用，但希望标明出处，感激不尽(●'◡'●)

[1]: https://leetcode-cn.com/problems/partition-labels/ "划分字母区间 | 力扣（LeetCode）"
[2]: https://leetcode-cn.com/problems/partition-labels/solution/hua-fen-zi-mu-qu-jian-by-leetcode-solution/ "划分字母区间 | 题解（LeetCode）"
