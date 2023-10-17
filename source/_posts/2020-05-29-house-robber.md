---
title: 打家劫舍
date: 2020-05-29 21:27:30
updated: 2020-05-29 21:27:30
categories:
    - LeetCode
tags:
    - LeetCode
    - Java
    - 学习笔记
    - 递归
    - 记忆化
    - 动态规划
    - 滚动数组
---
---

# 题目描述

你是一个专业的小偷，计划偷窃沿街的房屋。每间房内都藏有一定的现金，影响你偷窃的唯一制约因素就是相邻的房屋装有相互连通的防盗系统，如果两间相邻的房屋在同一晚上被小偷闯入，系统会自动报警。给定一个代表每个房屋存放金额的非负整数数组，计算你不触动警报装置的情况下 ，一夜之内能够偷窃到的最高金额。

**示例 1：**
> 输入：[1,2,3,1]
> 输出：4
> 解释：偷窃 1 号房屋 (金额 = 1) ，然后偷窃 3 号房屋 (金额 = 3)。偷窃到的最高金额 = 1 + 3 = 4 。

**示例 2：**
> 输入：[2,7,9,3,1]
> 输出：12
> 解释：偷窃 1 号房屋 (金额 = 2), 偷窃 3 号房屋 (金额 = 9)，接着偷窃 5 号房屋 (金额 = 1)。偷窃到的最高金额 = 2 + 9 + 1 = 12 。

**提示：**
* 0 <= nums.length <= 100
* 0 <= nums[i] <= 400

<!-- more -->

# 递归

对于此题，我们从起始位置，第一家开始出发，有两种情况，偷第一家，跳过第二家，然后从第三家开始继续选择（偷第三家或者第四家）；或者偷第二家，然后从第四家开始选择（偷第四家或者第五家）。选择两种情况的最大值。之后递归处理。

```java
public int rob(int[] nums) {
    if (nums == null || nums.length == 0) {
        return 0;
    } else if (nums.length == 1) {
        return nums[0];
    }
    return Math.max(nums[0] + rob(nums, 2), nums[1] + rob(nums, 3));
}

public int rob(int[] nums, int index) {
    if (index == nums.length - 1) {
        return nums[index];
    } else if (index > nums.length - 1) {
        return 0;
    } else {
        return Math.max(nums[index] + rob(nums, index + 2), nums[index + 1] + rob(nums, index + 3));
    }
}
```

上面是一个最容易理解的解法，但是会超时，原因是，描述中我们可以发现，两种情况都会计算从第四家开始偷的情况。后面递归也是一样的，导致了大量的重复计算。于是我们进行优化，使用 `int[] money` 数组表示从第i家计算能偷的最大值，递归的时候，同时记录递归中间值，这样我们当有重复计算的时候，比如上面所说的第二种情况偷第四家的时候，可以使用第一种情况计算好的从第四家开始偷的最大值。避免了重复递归计算。

```java
int[] money = null;

public int rob(int[] nums) {
    if (nums == null || nums.length == 0) {
        return 0;
    } else if (nums.length == 1) {
        return nums[0];
    }
    money = new int[nums.length];
    Arrays.fill(money, -1);
    return Math.max(nums[0] + rob(nums, 2), nums[1] + rob(nums, 3));
}

public int rob(int[] nums, int index) {
    if (index == nums.length - 1) {
        return nums[index];
    } else if (index > nums.length - 1) {
        return 0;
    } else {
        money[index] = money[index] == -1 ? nums[index] + rob(nums, index + 2) : money[index];
        money[index + 1] = money[index + 1] == -1 ? nums[index + 1] + rob(nums, index + 3) : money[index + 1];
        return Math.max(money[index], money[index + 1]);
    }
}
```

# 动态规划

首先考虑最简单的情况。如果只有一间房屋，则偷窃该房屋，可以偷窃到最高总金额。如果只有两间房屋，则由于两间房屋相邻，不能同时偷窃，只能偷窃其中的一间房屋，因此选择其中金额较高的房屋进行偷窃，可以偷窃到最高总金额。如果房屋数量大于两间，应该如何计算能够偷窃到的最高总金额呢？对于第 k (k > 2) 间房屋，有两个选项：
1. 偷窃第 k 间房屋，那么就不能偷窃第 k−1 间房屋，偷窃总金额为前 k−2 间房屋的最高总金额与第 k 间房屋的金额之和。
2. 不偷窃第 k 间房屋，偷窃总金额为前 k−1 间房屋的最高总金额。

在两个选项中选择偷窃总金额较大的选项，该选项对应的偷窃总金额即为前 k 间房屋能偷窃到的最高总金额。用 dp[i] 表示前 i 间房屋能偷窃到的最高总金额，那么就有如下的状态转移方程：
> dp[i] = max(dp[i − 2] + nums[i], dp[i − 1])

边界条件为：
> dp[0]=nums[0] 只有一间房屋，则偷窃该房屋
> dp\[1\]=max(nums[0], nums\[1\]) 只有两间房屋，选择其中金额较高的房屋进行偷窃

最终的答案即为 dp[n − 1]，其中 n 是数组的长度。

```java
public int rob(int[] nums) {
    if (nums == null || nums.length == 0) {
        return 0;
    }
    int length = nums.length;
    if (length == 1) {
        return nums[0];
    }
    int[] dp = new int[length];
    dp[0] = nums[0];
    dp[1] = Math.max(nums[0], nums[1]);
    for (int i = 2; i < length; i++) {
        dp[i] = Math.max(dp[i - 2] + nums[i], dp[i - 1]);
    }
    return dp[length - 1];
}
```

上述方法使用了数组存储结果。考虑到每间房屋的最高总金额只和该房屋的前两间房屋的最高总金额相关，因此可以使用滚动数组，在每个时刻只需要存储前两间房屋的最高总金额。

```java
public int rob(int[] nums) {
    if (nums == null || nums.length == 0) {
        return 0;
    }
    int length = nums.length;
    if (length == 1) {
        return nums[0];
    }
    int first = nums[0], second = Math.max(nums[0], nums[1]);
    for (int i = 2; i < length; i++) {
        int temp = second;
        second = Math.max(first + nums[i], second);
        first = temp;
    }
    return second;
}
```

# 来源

> [打家劫舍 | 力扣（LeetCode）][1]
> [打家劫舍 | 题解（LeetCode）][2]

---

[1]: https://leetcode-cn.com/problems/house-robber/ "打家劫舍 | 力扣（LeetCode）"
[2]: https://leetcode-cn.com/problems/house-robber/solution/da-jia-jie-she-by-leetcode-solution/ "打家劫舍 | 题解（LeetCode）"
