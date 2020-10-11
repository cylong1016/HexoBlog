---
title: 删除排序数组中的重复项
date: 2020-09-18 23:16:31
updated: 2020-09-18 23:16:31
categories:
    - LeetCode
tags:
    - leetcode
    - java
    - 学习笔记
---
---

# 题目描述

给定一个排序数组，你需要在原地删除重复出现的元素，使得每个元素只出现一次，返回移除后数组的新长度。不要使用额外的数组空间，你必须在原地修改输入数组 并在使用 O(1) 额外空间的条件下完成。

**示例1:**
> 给定数组 nums = [1, 1, 2], 
> 函数应该返回新的长度 2, 并且原数组 nums 的前两个元素被修改为 1, 2。 你不需要考虑数组中超出新长度后面的元素。

**示例2:**
> 给定 nums = [0, 0, 1, 1, 1, 2, 2, 3, 3, 4],
> 函数应该返回新的长度 5, 并且原数组 nums 的前五个元素被修改为 0, 1, 2, 3, 4。你不需要考虑数组中超出新长度后面的元素。

**说明:**
> 为什么返回数值是整数，但输出的答案是数组呢?
> 请注意，输入数组是以「引用」方式传递的，这意味着在函数里修改输入数组对于调用者是可见的。
> 你可以想象内部操作如下:
```java
// nums 是以“引用”方式传递的。也就是说，不对实参做任何拷贝
int len = removeDuplicates(nums);

// 在函数里修改输入数组对于调用者是可见的。
// 根据你的函数返回的长度, 它会打印出数组中该长度范围内的所有元素。
for (int i = 0; i < len; i++) {
    print(nums[i]);
}
```

<!-- more -->

# 题解

题目中是一个排序后的数组，那么相同的元素一定是排列在一起的，我们可以使用两个指针 i 和 j，我们不断的移动j指针，只要 nums[i]=nums[j]，我们就进行 j++ 操作，跳过重复项，直到 nums[i]≠nums[j] 的时候，说明遇到了下一个非重复项，于是我们就将 num[j] 的值复制到 num[i + 1] 的位置，接着重复此流程，遍历完全部数组。

```
public int removeDuplicates(int[] nums) {
    int i = 0;
    for (int j = 1; j < nums.length; j++) {
        if (nums[i] != nums[j]) {
            i++;
            nums[i] = nums[j];
        }
    }
    return i + 1;
}
```

## 复杂度分析

* 时间复杂度：O(n)，假设数组的长度是 n，那么 i 和 j 分别最多遍历 n 步。
* 空间复杂度：O(1)。

# 来源
> [删除排序数组中的重复项 | 力扣（LeetCode）][1]
> [删除排序数组中的重复项 | 题解（LeetCode）][2]

---

> 文章标题：<a href='{{ permalink }}' title='{{ title }}' >{{ title }}</a>
> 文章作者：[cylong](http://www.cylong.com/about/ "cylong")
> 文章链接：<a href='{{ permalink }}' title='{{ title }}' >{{ permalink }}</a>
> 有问题或者建议欢迎在下方评论。欢迎转载、引用，但希望标明出处，感激不尽(●'◡'●)

[1]: https://leetcode-cn.com/problems/remove-duplicates-from-sorted-array/ "删除排序数组中的重复项 | 力扣（LeetCode）"
[2]: https://leetcode-cn.com/problems/remove-duplicates-from-sorted-array/solution/shan-chu-pai-xu-shu-zu-zhong-de-zhong-fu-xiang-by-/ "删除排序数组中的重复项 | 题解（LeetCode）"