---
title: 恢复空格
date: 2020-07-09 15:31:11
updated: 2020-07-09 15:31:11
categories:
    - LeetCode
tags:
    - leetcode
    - java
    - 学习笔记
    - 动态规划
    - 字典树
    - 字符串
---
---

# 题目描述

哦，不！你不小心把一个长篇文章中的空格、标点都删掉了，并且大写也弄成了小写。像句子"I reset the computer. It still didn’t boot!"已经变成了"iresetthecomputeritstilldidntboot"。在处理标点符号和大小写之前，你得先把它断成词语。当然了，你有一本厚厚的词典dictionary，不过，有些词没在词典里。假设文章用sentence表示，设计一个算法，把文章断开，要求未识别的字符最少，返回未识别的字符数。

**示例：**

> 输入：
> dictionary = ["looked", "just", "like", "her", "brother"]
> sentence = "jesslookedjustliketimherbrother"
> 输出： 7
> 解释： 断句后为"jess looked just like tim her brother"，共7个未识别字符。

**提示：**
* 0 <= len(sentence) <= 1000
* dictionary中总字符数不超过 150000。
* 你可以认为dictionary和sentence中只包含小写字母。

<!-- more -->

# 动态规划

这里采用动态规划，创建一个数组 dp 用来记录结果。句子从前往后看，其中 `dp[0]=0` 表示句子是空字符串时没有未识别的字符，dp[i] 表示句子前 i 个字符中最少的未识别字符数。然后来找状态转移方程。对于前 i 个字符，即句子字符串的 [0,i)，它可能是由最前面的 [0,j) 子字符串加上一个字典匹配的单词得到，也就是 `dp[i]=dp[j]`, `j < i`；也可能没找到字典中的单词，可以用它前 `i - 1` 个字符的结果加上一个没有匹配到的第 i 个字符，即 `dp[i] = dp[i-1] + 1`。要注意的是，即使前面存在匹配的单词，也不能保证哪一种剩下的字符最少，所以每轮都要比较一次最小值。

```java
public int respace(String[] dictionary, String sentence) {
    List<String> dic = Arrays.asList(dictionary);
    int len = sentence.length();
    // dp[i]表示sentence前i个字符所得结果
    int[] dp = new int[len + 1];
    for (int i = 1; i <= len; i++) {
        dp[i] = dp[i - 1] + 1;  // 先假设当前字符作为单词不在字典中
        for (int j = 0; j < i; j++) {
            if (dic.contains(sentence.substring(j, i))) {
                dp[i] = Math.min(dp[i], dp[j]);
            }
        }
    }
    return dp[len];
}
```

## 复杂度分析

* 时间复杂度：Ο(n²)，其中 n 是字符串长度。
* 空间复杂度：O(n)，其中 n 是字符串长度，保存dp的中间值。

# 字典树

这里重点讲述Trie字典树的解法。首先看一个字典树的例子：

![字典树](字典树.png)

该树包含的单词集合为 {“at”, “bee”, “ben”, “bt”, “q”}。每一个节点保存一个字符，因为题目说只包含小写字母，所以一个节点最多可以有 26 个子节点。每次查找单词都从空白的根节点开始，比如查找单词 "cat"，第一个字符 'c' 就不存在，直接返回 false；查找单词 "bee"，根节点下有 b，b 的子节点有 e，下面还有 e 所以查到了。但是如果查找单词"be",同样的方法 'b' 和 'e' 都存在，但是字典里没有 "be" 这个单词，所以在树里还需要一个 boolean 变量表示当前节点是不是一个单词的结尾，如图绿色表示。如果往字典中插入一个 "be" 单词，此时 b 节点下的 e 节点也应该标绿，此时再查找 "be"，在 e 节点发现它是个单词，所以返回 true。

使用字典树可利用字符串的公共前缀来减少查询时间，最大限度地减少无谓的字符串比较。

```java
public int respace(String[] dictionary, String sentence) {
    root = new TrieNode();
    makeTrie(dictionary);   //创建字典树
    int n = sentence.length();
    int[] dp = new int[n + 1];
    //这里从sentence最后一个字符开始
    for (int i = n - 1; i >= 0; i--) {
        dp[i] = n - i;    //初始默认后面全不匹配
        TrieNode node = root;
        for (int j = i; j < n; j++) {
            int c = sentence.charAt(j) - 'a';
            if (node.childs[c] == null) {
                //例如"abcde",i=1,j=2 可找出长度关系
                dp[i] = Math.min(dp[i], j - i + 1 + dp[j + 1]);
                break;
            }
            if (node.childs[c].isWord) {
                dp[i] = Math.min(dp[i], dp[j + 1]);
            } else {
                dp[i] = Math.min(dp[i], j - i + 1 + dp[j + 1]);
            }
            node = node.childs[c];
        }
    }
    return dp[0];
}

public void makeTrie(String[] dictionary) {
    for (String str : dictionary) {
        TrieNode node = root;
        for (int k = 0; k < str.length(); k++) {
            int i = str.charAt(k) - 'a';
            if (node.childs[i] == null) {
                node.childs[i] = new TrieNode();
            }
            node = node.childs[i];
        }
        node.isWord = true; //单词的结尾
    }
}

/**
 * 自定义一个TrieNode类型。
 * 这里不用建一个变量来存当前节点表示的字符，
 * 因为只要该节点不为null，就说明存在这个字符
 */
class TrieNode {
    TrieNode[] childs;
    boolean isWord;

    public TrieNode() {
        childs = new TrieNode[26];
        isWord = false;
    }
}
```

# 来源
> [恢复空格 | 力扣（LeetCode）][1]
> [恢复空格 | 题解（LeetCode）][2]

---

> 文章标题：<a href='{{ permalink }}' title='{{ title }}' >{{ title }}</a>
> 文章作者：[cylong](http://www.cylong.com/about/ "cylong")
> 文章链接：<a href='{{ permalink }}' title='{{ title }}' >{{ permalink }}</a>
> 有问题或者建议欢迎在下方评论。欢迎转载、引用，但希望标明出处，感激不尽(●'◡'●)

[1]: https://leetcode-cn.com/problems/re-space-lcci/ "恢复空格 | 力扣（LeetCode）"
[2]: https://leetcode-cn.com/problems/re-space-lcci/solution/cong-bao-li-ru-shou-you-hua-yi-ji-triezi-dian-shu-/ "恢复空格 | 题解（LeetCode）"
