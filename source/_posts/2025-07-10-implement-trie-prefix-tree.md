---
title: 实现 Trie (前缀树)
date: 2025-07-10 23:06:58
updated: 2025-07-10 23:06:58
categories:
    - LeetCode
tags:
    - LeetCode
    - LeetCode中等
    - Java
    - 学习笔记
    - 树
    - 字典树
    - 字符串
---
---

# 题目描述

`Trie`（发音类似 "try"）或者说 **前缀树** 是一种树形数据结构，用于高效地存储和检索字符串数据集中的键。这一数据结构有相当多的应用情景，例如自动补全和拼写检查。请你实现 `Trie` 类：

* `Trie()` 初始化前缀树对象。
* `void insert(String word)` 向前缀树中插入字符串 `word` 。
* `boolean search(String word)` 如果字符串 `word` 在前缀树中，返回 `true`（即，在检索之前已经插入）；否则，返回 `false` 。
* `boolean startsWith(String prefix)` 如果之前已经插入的字符串 `word` 的前缀之一为 `prefix` ，返回 `true` ；否则，返回 `false` 。

**示例 1:**
> **输入：**
> `["Trie", "insert", "search", "search", "startsWith", "insert", "search"]`
> `[[], ["apple"], ["apple"], ["app"], ["app"], ["app"], ["app"]]`
> **输出：**
> `[null, null, true, false, true, null, true]`
> **解释：**
> `Trie trie = new Trie();`
> `trie.insert("apple");`
> `trie.search("apple");`   // 返回 True
> `trie.search("app");`     // 返回 False
> `trie.startsWith("app");` // 返回 True
> `trie.insert("app");`
> `trie.search("app");`     // 返回 True

**提示:**
* `1 <= word.length, prefix.length <= 2000`
* `word` 和 `prefix` 仅由小写英文字母组成
* `insert`、`search` 和 `startsWith` 调用次数总计不超过 `3 * 10^4` 次

<!-- more -->

# 基于数组的 Trie 实现

前缀树（`Trie`）是一种高效检索字符串数据集中的键的树形数据结构。它广泛应用于搜索引擎、拼写检查、自动补全等场景。`Trie` 的核心思想是利用字符串的公共前缀来减少查询时间，最大限度地减少无谓的字符串比较。

## Trie 节点设计

```java
class Node {
    // 子节点数组，每个位置对应一个字母（a-z）
    Node[] son = new Node[26];
    // 标记当前节点是否为单词结尾
    boolean end;
}
```

## Trie 类实现

```java
class Trie {
    // 根节点，不存储实际字符
    private final Node root = new Node();

    public Trie() {}
    
    /**
     * 向 Trie 中插入一个单词
     * 
     * @param word 要插入的单词
     */
    public void insert(String word) {
        // 从根节点开始
        Node cur = root;
        for (char c : word.toCharArray()) {
            // 将字符转换为数组索引（0-25）
            c -= 'a';
            // 如果当前字符对应的路径不存在
            if (cur.son[c] == null) {
                // 创建新节点
                cur.son[c] = new Node();
            }
            // 移动到子节点
            cur = cur.son[c];
        }
        // 标记单词结束
        cur.end = true;
    }
    
    /**
     * 搜索 Trie 中是否存在完整单词
     * 
     * @param word 要搜索的单词
     * @return 存在返回 true，否则返回 false
     */
    public boolean search(String word) {
        // 需要完全匹配（状态值 2）
        return find(word) == 2;
    }
    
    /**
     * 检查 Trie 中是否有以指定前缀开头的单词
     * 
     * @param prefix 要检查的前缀
     * @return 存在返回 true，否则返回 false
     */
    public boolean startsWith(String prefix) {
        // 只要路径存在即可（状态值 1 或 2）
        return find(prefix) != 0;
    }
    
    /**
     * 内部查找方法，返回匹配状态
     * 
     * @param word 要查找的单词或前缀
     * @return 0: 未找到, 1: 前缀存在, 2: 完整单词存在
     */
    private int find(String word) {
        Node cur = root;
        for (char c : word.toCharArray()) {
            // 字符转索引
            c -= 'a';
            if (cur.son[c] == null) {
                // 路径中断，未找到
                return 0;
            }
            // 继续向下查找
            cur = cur.son[c];
        }
        // 路径存在，检查是否为完整单词
        return cur.end ? 2 : 1;
    }
}
```

## 复杂度分析

* **时间复杂度：**
    * 插入操作：`O(L)`，其中 `L` 是插入单词的长度。需要遍历单词的每个字符。
    * 搜索操作：`O(L)`，其中 `L` 是搜索单词的长度。需要遍历单词的每个字符。
    * 前缀搜索：`O(L)`，其中 `L` 是前缀的长度。需要遍历前缀的每个字符。
* **空间复杂度：**
    * 最坏情况：`O(MN)`，其中 `M` 是单词的平均长度，`N` 是插入的单词数量。每个字符都需要一个节点。
    * 最佳情况：当单词共享大量前缀时，空间复杂度会显著降低。

# 总结

1. **节点结构：**每个节点包含一个长度为 `26` 的子节点数组（对应 `26` 个小写字母）和一个结束标志。
2. **路径创建：**在插入过程中，如果路径不存在则动态创建新节点。
3. **结束标志：**单词插入完成后，在最后一个节点标记结束标志。
4. **查找优化：**使用统一的 `find` 方法处理完整单词查找和前缀查找，避免代码重复。
5. **字符转换：**通过 `c - 'a'` 将字符转换为数组索引（0-25），高效访问子节点。

`Trie` 是一种高效处理字符串相关问题的数据结构，特别适合前缀匹配的场景。本文实现的 `Trie` 结构清晰，操作高效，时间复杂度均为线性级别。理解 `Trie` 的工作原理对于解决字符串搜索、自动补全等问题至关重要。实际应用中，可以根据需求扩展 `Trie` 功能，如添加词频统计、删除操作等。

# 来源

> [208. 实现 Trie (前缀树) | 力扣（LeetCode）][1]
> [208. 实现 Trie (前缀树) | 题解 | 灵茶山艾府][2]

---

[1]: https://leetcode.cn/problems/implement-trie-prefix-tree/description/ "208. 实现 Trie (前缀树) | 力扣（LeetCode）"
[2]: https://leetcode.cn/problems/implement-trie-prefix-tree/solutions/2993894/cong-er-cha-shu-dao-er-shi-liu-cha-shu-p-xsj4/ "208. 实现 Trie (前缀树) | 题解 | 灵茶山艾府"
