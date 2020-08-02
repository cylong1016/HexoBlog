---
title: 盛最多水的容器
date: 2020-01-16 17:04:09
updated: 2020-01-16 17:04:09
categories:
    - LeetCode
tags:
    - leetcode
    - java
    - 学习笔记
    - 指针
    - 双指针
    - 数组
    - 几何
---
---

# 题目描述

给你 n 个非负整数 a1，a2，...，an，每个数代表坐标中的一个点 (i, ai) 。在坐标内画 n 条垂直线，垂直线 i 的两个端点分别为 (i, ai) 和 (i, 0)。找出其中的两条线，使得它们与 x 轴共同构成的容器可以容纳最多的水。

**说明：** 你不能倾斜容器，且 n 的值至少为 2。

![盛水最多的容器描述](盛水最多的容器描述.jpg)

图中垂直线代表输入数组 [1, 8, 6, 2, 5, 4, 8, 3, 7]。在此情况下，容器能够容纳水（表示为蓝色部分）的最大值为 49。

**示例：**
> 输入：[1, 8, 6, 2, 5, 4, 8, 3, 7]
> 输出：49

<!-- more -->

# 题解

通过题目描述的图中可以容易看出来，我们既然想盛更多的水，那么我们需要让垂直x轴的两条线尽量的高，也让这两条线距离尽量的远。两条线最大的距离就是数组的长度，于是我们使用双指针 `i，j 且 i < j` 分别指向数组的两端，作为初始的容器大小 `Math.min(height[i], height[j]) * (j - i)`。接下来，我们就要移动左右指针，移动哪个呢？我们直觉上，要移动指针指的值较小的一个。因为我们既然移动了指针，那么肯定会让 `j - i` 变小，那么如果我们想要让容器盛水更多，那么肯定要让 `Math.min(height[i], height[j])` 更大才行。否则如果我们移动指针指的值较大的一个，那么最后计算出来的 `Math.min(height[i], height[j])` 肯定小于等于原有的值，那么盛水也将小于等于原有的值。我们不需要这样的结果。最后，我们只要取每次指针移动后，计算值中最大的值，即为盛水的最大值。更详细的介绍可以参考[Leetcode官方题解][2]。

```java
public int maxArea(int[] height) {
    if (height == null || height.length < 2) {
        return 0;
    }
    int result = 0;
    int i = 0;
    int j = height.length - 1;
    while (i < j) {
        result = Math.max(Math.min(height[i], height[j]) * (j - i), result);
        if (height[i] < height[j]) {
            i++;
        } else {
            j--;
        }
    }
    return result;
}
```

以下只是看到了有大佬写的更简单的代码，拿来学习下。

```java
public int maxArea(int[] height) {
    if (height == null || height.length < 2) {
        return 0;
    }
    int result = 0;
    int i = 0;
    int j = height.length - 1;
    while (i < j) {
        int minH = height[i] < height[j] ? height[i++] : height[j--];
        result = Math.max(minH * (j - i + 1), result);
    }
    return result;
}
```

# 复杂度分析

* 时间复杂度：O(N)，双指针总计最多遍历整个数组一次。
* 空间复杂度：O(1)，只需要额外的常数级别的空间。

# 来源
> [盛最多水的容器 | 力扣（LeetCode）][1]
> [盛最多水的容器 | 题解（LeetCode）][2]

---

> 文章标题：<a href='{{ permalink }}' title='{{ title }}' >{{ title }}</a>
> 文章作者：[cylong](http://www.cylong.com/about/ "cylong")
> 文章链接：<a href='{{ permalink }}' title='{{ title }}' >{{ permalink }}</a>
> 有问题或者建议欢迎在下方评论。欢迎转载、引用，但希望标明出处，感激不尽(●'◡'●)

[1]: https://leetcode-cn.com/problems/container-with-most-water/ "盛最多水的容器 | 力扣（LeetCode）"
[2]: https://leetcode-cn.com/problems/container-with-most-water/solution/sheng-zui-duo-shui-de-rong-qi-by-leetcode-solution/ "盛最多水的容器 | 题解（LeetCode）"
