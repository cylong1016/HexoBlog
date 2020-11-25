---
title: 除数博弈
date: 2020-07-24 16:19:07
categories:
    - LeetCode
tags:
    - leetcode
    - java
    - 学习笔记
    - 数学归纳法
---
---

# 题目描述

爱丽丝和鲍勃一起玩游戏，他们轮流行动。爱丽丝先手开局。最初，黑板上有一个数字 N 。在每个玩家的回合，玩家需要执行以下操作：

* 选出任一 x，满足 0 < x < N 且 N % x == 0 。
* 用 N - x 替换黑板上的数字 N 。

如果玩家无法执行这些操作，就会输掉游戏。只有在爱丽丝在游戏中取得胜利时才返回 True，否则返回 False。假设两个玩家都以最佳状态参与游戏。

**示例 1：**
> 输入：2
> 输出：true
> 解释：爱丽丝选择 1，鲍勃无法进行操作。

**示例 2：**
> 输入：3
> 输出：false
> 解释：爱丽丝选择 1，鲍勃也选择 1，然后爱丽丝无法进行操作。

**提示：**
* 1 <= N <= 1000

<!-- more -->

# 归纳法

博弈类的问题常常让我们摸不着头脑。当我们没有解题思路的时候，不妨试着写几项试试：

* N = 1 的时候，区间 (0, 1) 中没有整数是 n 的因数，所以此时 Alice 败。
* N = 2 的时候，Alice 只能拿 1，N 变成 1，Bob 无法继续操作，故 Alice 胜。
* N = 3 的时候，Alice 只能拿 1，N 变成 2，根据 N = 2 的结论，我们知道此时 Bob 会获胜，Alice 败。
* N = 4 的时候，Alice 能拿 1 或 2，如果 Alice 拿 1，根据 N = 3 的结论，Bob 会失败，Alice 会获胜。
* N = 5 的时候，Alice 只能拿 1，根据 N = 4 的结论，Alice 会失败。
* ......

写到这里，也许你有了一些猜想。没关系，请大胆地猜想，在这种情况下大胆地猜想是 AC 的第一步。也许你会发现这样一个现象：N 为奇数的时候 Alice（先手）必败，N 为偶数的时候 Alice 必胜。 这个猜想是否正确呢？下面我们来想办法证明它。

**证明**
1. N = 1 和 N = 2 时结论成立。
2. N > 2 时，假设 N ≤ k 时该结论成立，则 N = k + 1 时：
  * 如果 k 为偶数，则 k + 1 为奇数，x 是 k + 1 的因数，只可能是奇数，而奇数减去奇数等于偶数，且 k + 1 - x ≤ k，故轮到 Bob 的时候都是偶数。而根据我们的猜想假设 N ≤ k 的时候偶数的时候先手必胜，故此时无论 Alice 拿走什么，Bob 都会处于必胜态，所以 Alice 处于必败态。
  * 如果 k 为奇数，则 k + 1 为偶数，x 可以是奇数也可以是偶数，若 Alice 减去一个奇数，那么 k + 1 - x 是一个小于等于 k 的奇数，此时 Bob 占有它，处于必败态，则 Alice 处于必胜态。

综上所述，这个猜想是正确的。

```java
bool divisorGame(int N) {
    return N % 2 == 0;
}
```

## 复杂度分析

* 时间复杂度：Ο(1)。
* 空间复杂度：O(1)。

# 递推

上述方法中，我们写出了前面几项的答案，在这个过程中我们发现，Alice 处在 N = k 的状态时，他（她）做一步操作，必然使得 Bob 处于 N = m (m < k) 的状态。因此我们只要看是否存在一个 m 是必败的状态，那么 Alice 直接执行对应的操作让当前的数字变成 m，Alice 就必胜了，如果没有任何一个是必败的状态的话，说明 Alice 无论怎么进行操作，最后都会让 Bob 处于必胜的状态，此时 Alice 是必败的。

结合以上我们定义 f[i] 表示当前数字 i 的时候先手是处于必胜态还是必败态，true 表示先手必胜，false 表示先手必败，从前往后递推，根据我们上文的分析，枚举 i 在 (0, i) 中 i 的因数 j，看是否存在 f[i-j] 为必败态即可。

```java
public boolean divisorGame(int N) {
    boolean[] dp = new boolean[N + 2];
    dp[1] = false;
    dp[2] = true;
    for (int i = 3; i <= N; i++) {
        for (int j = 1; j < i; j++) {
            if ((i % j == 0) && !dp[i - j]) {
                dp[i] = true;
                break;
            }
        }
    }
    return dp[N];
}
```

## 复杂度分析

* 时间复杂度：Ο(n²)，递推的时候一共有 n 个状态要计算，每个状态需要 O(n) 的时间枚举因数。
* 空间复杂度：O(n)，我们需要 O(n) 的空间存储递推数组 f 的值。

# 来源

> [除数博弈 | 力扣（LeetCode）][1]
> [除数博弈 | 题解（LeetCode）][2]

---

> 文章标题：<a href='{{ permalink }}' title='{{ title }}' >{{ title }}</a>
> 文章作者：[cylong](http://www.cylong.com/about/ "cylong")
> 文章链接：<a href='{{ permalink }}' title='{{ title }}' >{{ permalink }}</a>
> 有问题或者建议欢迎在下方评论。欢迎转载、引用，但希望标明出处，感激不尽(●'◡'●)

[1]: https://leetcode-cn.com/problems/divisor-game/ "除数博弈 | 力扣（LeetCode）"
[2]: https://leetcode-cn.com/problems/divisor-game/solution/chu-shu-bo-yi-by-leetcode-solution/ "除数博弈 | 题解（LeetCode）"
