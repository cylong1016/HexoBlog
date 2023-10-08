---
title: 有效的括号
date: 2020-07-27 16:59:21
updated: 2020-07-27 16:59:21
categories:
    - LeetCode
tags:
    - LeetCode
    - Java
    - 学习笔记
    - 栈
    - 字符串
---
---

# 题目描述

给定一个只包括 '('，')'，'{'，'}'，'['，']' 的字符串，判断字符串是否有效。有效字符串需满足：

* 左括号必须用相同类型的右括号闭合。
* 左括号必须以正确的顺序闭合。

**注意：** 空字符串可被认为是有效字符串。

**示例 1:**
> 输入: "()"
> 输出: true

**示例 2:**
> 输入: "()[]{}"
> 输出: true

**示例 3:**
> 输入: "(]"
> 输出: false

**示例 4:**
> 输入: "([)]"
> 输出: false

**示例 5:**
> 输入: "{[]}"
> 输出: true

<!-- more -->

# 题解

括号匹配是典型的代码分析问题，我们遍历字符串，每次处理一个括号，使用栈来保存这个括号。同时我们使用一个map来保存三种括号的开括号和闭括号。每次处理当前括号的时候，我们判断当前栈顶的元素是否是此括号对应的开括号，是的话，我们将弹出栈顶元素。否则我们将当前括号入栈。最后，如果栈的元素为空，那么可知此字符串是有效的字符串。

```java
public boolean isValid(String s) {
    if (s == null || s.length() == 0) {
        return true;
    }
    Stack<Character> parenthesesStack = new Stack<>();
    HashMap<Character, Character> parenthesesMap = new HashMap<>();
    parenthesesMap.put('(', ')');
    parenthesesMap.put('[', ']');
    parenthesesMap.put('{', '}');
    for (char parentheses : s.toCharArray()) {
        if (!parenthesesStack.isEmpty() && Character.valueOf(parentheses)
                .equals(parenthesesMap.get(parenthesesStack.peek()))) {
            parenthesesStack.pop();
        } else {
            parenthesesStack.push(parentheses);
        }
    }
    return parenthesesStack.isEmpty();
}
```

## 复杂度分析

* 时间复杂度：O(n)，因为我们一次只遍历给定的字符串中的一个字符并在栈上进行 O(1) 的推入和弹出操作。
* 空间复杂度：O(n)，当我们将所有的开括号都推到栈上时以及在最糟糕的情况下，我们最终要把所有括号推到栈上。例如 ((((((((((。

# 来源

> [有效的括号 | 力扣（LeetCode）][1]
> [有效的括号 | 题解（LeetCode）][2]

---

> 文章标题：<a href='{{ permalink }}' title='{{ title }}' >{{ title }}</a>
> 文章作者：[cylong](http://www.cylong.com/about/ "cylong")
> 文章链接：<a href='{{ permalink }}' title='{{ title }}' >{{ permalink }}</a>
> 有问题或者建议欢迎在下方评论。欢迎转载、引用，但希望标明出处，感激不尽(●'◡'●)

[1]: https://leetcode-cn.com/problems/valid-parentheses/ "有效的括号 | 力扣（LeetCode）"
[2]: https://leetcode-cn.com/problems/valid-parentheses/solution/you-xiao-de-gua-hao-by-leetcode/ "有效的括号 | 题解（LeetCode）"
