---
title: 字符串转换整数
date: 2019-12-21 00:51:08
updated: 2020-09-04 22:34:07
categories:
    - LeetCode
tags:
    - leetcode
    - java
    - 学习笔记
    - 字符串
    - 整数
    - 有限状态机
---
---

# 题目描述

请你来实现一个 atoi 函数，使其能将字符串转换成整数。首先，该函数会根据需要丢弃无用的开头空格字符，直到寻找到第一个非空格的字符为止。接下来的转化规则如下：

* 如果第一个非空字符为正或者负号时，则将该符号与之后面尽可能多的连续数字字符组合起来，形成一个有符号整数。
* 假如第一个非空字符是数字，则直接将其与之后连续的数字字符组合起来，形成一个整数。
* 该字符串在有效的整数部分之后也可能会存在多余的字符，那么这些字符可以被忽略，它们对函数不应该造成影响。

**注意：** 假如该字符串中的第一个非空格字符不是一个有效整数字符、字符串为空或字符串仅包含空白字符时，则你的函数不需要进行转换，即无法进行有效转换。在任何情况下，若函数不能进行有效的转换时，请返回 0 。

**提示：**

* 本题中的空白字符只包括空格字符 ' ' 。
* 假设我们的环境只能存储 32 位大小的有符号整数，那么其数值范围为 [−2³¹,  2³¹ − 1]。如果数值超过这个范围，请返回  INT_MAX (2³¹ − 1) 或 INT_MIN (−2³¹) 。
 

**示例 1:**
> 输入: "42"
> 输出: 42

**示例 2:**
> 输入: "   -42"
> 输出: -42
> 解释: 第一个非空白字符为 '-', 它是一个负号。我们尽可能将负号与后面所有连续出现的数字组合起来，最后得到 -42 。

**示例 3:**
> 输入: "4193 with words"
> 输出: 4193
> 解释: 转换截止于数字 '3' ，因为它的下一个字符不为数字。

**示例 4:**
> 输入: "words and 987"
> 输出: 0
> 解释: 第一个非空字符是 'w', 但它不是数字或正、负号。因此无法执行有效的转换。

**示例 5:**
> 输入: "-91283472332"
> 输出: -2147483648
> 解释: 数字 "-91283472332" 超过 32 位有符号整数范围。因此返回 INT_MIN (−2³¹) 。

<!-- more -->

# 正常遍历

正常遍历字符串，首先去掉空格，然后用flag记录正负，接下来遍历字符串中的数字，不断的转换为整数，直到遍历到无用字符为止。考虑到溢出的情况。我们在计算result的时候，先判断计算后的值是否对int行溢出了，使用表达式 `(Integer.MAX_VALUE - (c - '0')) * 1.0 / 10 >= result` 判断即可。

```java
public int myAtoi(String str) {
    str = str.trim();
    boolean flag = false;
    int result = 0;
    if (str.isEmpty()) {
        return 0;
    } else if (str.charAt(0) == '-') {
        flag = true;
    } else if (Character.isDigit(str.charAt(0))) {
        result += str.charAt(0) - '0';
    } else if (str.charAt(0) != '+') {
        return 0;
    }
    for (int i = 1; i < str.length(); i++) {
        char c = str.charAt(i);
        if (Character.isDigit(c)) {
            if ((Integer.MAX_VALUE - (c - '0')) * 1.0 / 10 >= result) {
                result = result * 10 + c - '0';
            } else {
                return flag ? Integer.MIN_VALUE : Integer.MAX_VALUE;
            }
        } else {
            break;
        }
    }
    return flag ? -result : result;
}
```

## 复杂度分析

* 时间复杂度：O(n)，其中 n 为字符串的长度。我们只需要依次处理所有的字符，处理每个字符需要的时间为 O(1)。
* 空间复杂度：Ο(1)。

# 有限状态机

针对有限状态机的学习还需要深入，先参考下官方的解答：[字符串转换整数 | 题解][2]

**2020-09-04 更新**
时隔这么久，终于静下心来研究了下有限状态机的解法。有限状态机在遇到类似“判断一个字符串，是否满足某种规则”这种问题的时候，可以非常有条理的去解决。有限状态机的主要思想是，在程序运行的时候有一个状态 state，我们从当前输入中取出一个字符，根据当前字符的类型，转移到下一个状态。我们还需要定义一个初始状态和结束状态，这样我们只要建立一个覆盖所有状态的表格，就可以解决本题的问题。先看下官方题解的状态机：

{% asset_img 有限状态机.png 有限状态机 %}

我们建立的表格如下（为了配合下面自己的代码，我把上面的状态做了微小修改）：

| 状态\字符类型 | space | +/-  | number | other |
|:-----------:|:-----:|:----:|:------:|:-----:|
| space       | space | sign | number | end   |
| sign        | end   | end  | number | end   |
| number      | end   | end  | number | end   |
| end         | end   | end  | end    | end   |

接下来我们写代码只要构造这个 stateTable ，然后遍历字符串并不断更新状态即可。对于本题，我们要在遇到数字的时候进行计算，遇到符号的时候记录正负。

```java

// 记录当前状态，初始为空格状态
State state = State.SPACE;
int sign = 1;
long ans = 0;

// 构造表格，记录当前状态遇到某个类型字符可以转移到的下一个状态
Map<State, Map<CharType, State>> stateTable = new HashMap<>();

{
    Map<CharType, State> spaceMap = new HashMap<CharType, State>() {{
        put(CharType.SPACE, State.SPACE);
        put(CharType.SIGN, State.SIGN);
        put(CharType.NUMBER, State.NUMBER);
        put(CharType.OTHER, State.END);
    }};
    Map<CharType, State> signMap = new HashMap<CharType, State>() {{
        put(CharType.SPACE, State.END);
        put(CharType.SIGN, State.END);
        put(CharType.NUMBER, State.NUMBER);
        put(CharType.OTHER, State.END);
    }};
    Map<CharType, State> numberMap = new HashMap<CharType, State>() {{
        put(CharType.SPACE, State.END);
        put(CharType.SIGN, State.END);
        put(CharType.NUMBER, State.NUMBER);
        put(CharType.OTHER, State.END);
    }};
    Map<CharType, State> otherMap = new HashMap<CharType, State>() {{
        put(CharType.SPACE, State.END);
        put(CharType.SIGN, State.END);
        put(CharType.NUMBER, State.END);
        put(CharType.OTHER, State.END);
    }};
    stateTable.put(State.SPACE, spaceMap);
    stateTable.put(State.SIGN, signMap);
    stateTable.put(State.NUMBER, numberMap);
    stateTable.put(State.END, otherMap);
}

public int myAtoi(String str) {
    for (char c : str.toCharArray()) {
        changeState(c);
    }
    return (int)ans * sign;
}

public void changeState(char c) {
    state = stateTable.get(state).get(getCharType(c));
    if (state == State.NUMBER) {
        ans = ans * 10 + (c - '0');
        ans = sign == 1 ? Math.min(ans, Integer.MAX_VALUE) : Math.min(ans, -(long) Integer.MIN_VALUE);
    } else if (state == State.SIGN) {
        sign = c == '-' ? -sign : sign;
    }
}

public CharType getCharType(char c) {
    if (c == ' ') {
        return CharType.SPACE;
    } else if (c == '+' || c == '-') {
        return CharType.SIGN;
    } else if (Character.isDigit(c)) {
        return CharType.NUMBER;
    } else {
        return CharType.OTHER;
    }
}

enum State {
    SPACE,
    SIGN,
    NUMBER,
    END
}

enum CharType {
    SPACE,
    SIGN,
    NUMBER,
    OTHER
}
```

## 复杂度分析

* 时间复杂度：O(n)，其中 n 为字符串的长度。我们只需要依次处理所有的字符，处理每个字符需要的时间为 O(1)。
* 空间复杂度：Ο(1)，自动机的状态只需要常数空间存储。

# 来源
> [字符串转换整数 | 力扣（LeetCode）][1]
> [字符串转换整数 | 题解（LeetCode）][2]

---

> 文章标题：<a href='{{ permalink }}' title='{{ title }}' >{{ title }}</a>
> 文章作者：[cylong](http://www.cylong.com/about/ "cylong")
> 文章链接：<a href='{{ permalink }}' title='{{ title }}' >{{ permalink }}</a>
> 有问题或者建议欢迎在下方评论。欢迎转载、引用，但希望标明出处，感激不尽(●'◡'●)

[1]: https://leetcode-cn.com/problems/string-to-integer-atoi/ "字符串转换整数 | 力扣（LeetCode）"
[2]: https://leetcode-cn.com/problems/string-to-integer-atoi/solution/zi-fu-chuan-zhuan-huan-zheng-shu-atoi-by-leetcode-/ "字符串转换整数 | 题解（LeetCode）"
