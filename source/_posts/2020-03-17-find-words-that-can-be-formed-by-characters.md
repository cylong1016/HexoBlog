---
title: 拼写单词
date: 2020-03-17 22:29:14
updated: 2020-03-17 22:29:14
categories:
    - LeetCode
tags:
    - leetcode
    - java
    - 学习笔记
    - 数组
    - 字符串
---
---

# 题目描述

给你一份『词汇表』（字符串数组） words 和一张『字母表』（字符串） chars。假如你可以用 chars 中的『字母』（字符）拼写出 words 中的某个『单词』（字符串），那么我们就认为你掌握了这个单词。返回词汇表 words 中你掌握的所有单词的长度之和。

**注意：每次拼写（指拼写词汇表中的一个单词）时，chars 中的每个字母都只能用一次。**

**示例 1：**
> 输入：words = ["cat", "bt", "hat", "tree"], chars = "atach"
> 输出：6
> 解释：可以形成字符串 "cat" 和 "hat"，所以答案是 3 + 3 = 6。

**示例 2：**
> 输入：words = ["hello", "world", "leetcode"], chars = "welldonehoneyr"
> 输出：10
> 解释：可以形成字符串 "hello" 和 "world"，所以答案是 5 + 5 = 10。

**提示：**
* 1 <= words.length <= 1000
* 1 <= words[i].length, chars.length <= 100
* 所有字符串中都仅包含小写英文字母

<!-- more -->

# 题解

最简单的方法，暴力方法，我们遍历全部的word，判断word的中的所有字符是否都在chars中存在，同时我们标记下已经被记录的字符，防止重复使用。

```java
public int countCharacters(String[] words, String chars) {
    int length = 0;
    char[] charArr = chars.toCharArray();
    for (String word : words) {
        // 判断chars中是否包含word
        if (findWord(word, charArr)) {
            length += word.length();
        }
    }
    return length;
}

public boolean findWord(String word, char[] charArr) {
    boolean[] flag = new boolean[charArr.length];
    char[] wordArr = word.toCharArray();
    for (int i = 0; i < wordArr.length; i++) {
        // 从chars中找到word中的字符
        if (!findChar(wordArr[i], charArr, flag)) {
            return false;
        }
    }
    return true;
}

public boolean findChar(char c, char[] charArr, boolean[] flag) {
    for (int i = 0; i < charArr.length; i++) {
        if (charArr[i] == c && !flag[i]) {
            // 保证每个字符只用到一次
            flag[i] = true;
            return true;
        }
    }
    return false;
}
```

## 复杂度分析

* 时间复杂度：Ο(M x N)，M为words中所有字符数，N为chars字符数。

# 进阶

显然，对于一个单词 word，只要其中的每个字母的数量都不大于 chars 中对应的字母的数量，那么就可以用 chars 中的字母拼写出 word。由于题目中的限制条件是小写英文字母，所以我们只要使用int[26]数组，分别保存字母表中的字母出现次数和word中字母出现的次数即可。

{% asset_img 图解.gif 统计字母出现次数 %}

```java
public int countCharacters(String[] words, String chars) {
    int len = 0;
    // 统计字母表中字符出现的次数
    int[] charNum = getCharNum(chars);
    for (String word : words) {
        // 统计word中字符出现的次数
        int[] wordNum = getCharNum(word);
        if (isContain(charNum, wordNum)) {
            len += word.length();
        }
    }
    return len;
}

private boolean isContain(int[] charNum, int[] wordNum) {
    for (int i = 0; i < wordNum.length; i++) {
        // 判断如果word中的字符数大于chars中的字符数，则无法拼写单词
        if (charNum[i] < wordNum[i]) {
            return false;
        }
    }
    return true;
}

private int[] getCharNum(String word) {
    int[] charNum = new int[26];
    for (char c : word.toCharArray()) {
        charNum[c - 'a']++;
    }
    return charNum;
}
```

## 复杂度分析

* 时间复杂度：Ο(n)，其中 n 为所有字符串的长度和。我们需要遍历每个字符串，包括 chars 以及数组 words 中的每个单词。
* 空间复杂度：Ο(S)，其中 S 为字符集大小，在本题中 S 的值为 26（所有字符串仅包含小写字母）。程序运行过程中，最多同时存在两个统计字符数的数组，使用的空间均不超过字符集大小 S，因此空间复杂度为 O(S)。

# 来源

> [拼写单词 | 力扣（LeetCode）][1]
> [拼写单词 | 题解（LeetCode）][2]

---

> 文章标题：<a href='{{ permalink }}' title='{{ title }}' >{{ title }}</a>
> 文章作者：[cylong](http://www.cylong.com/about/ "cylong")
> 文章链接：<a href='{{ permalink }}' title='{{ title }}' >{{ permalink }}</a>
> 有问题或者建议欢迎在下方评论。欢迎转载、引用，但希望标明出处，感激不尽(●'◡'●)

[1]: https://leetcode-cn.com/problems/find-words-that-can-be-formed-by-characters/ "二叉搜索树的第k大节点 | 力扣（LeetCode）"
[2]: https://leetcode-cn.com/problems/find-words-that-can-be-formed-by-characters/solution/tong-ji-zi-mu-chu-xian-de-ci-shu-shu-zu-ji-qiao-cj/ "二叉搜索树的第k大节点 | 题解（LeetCode）"
