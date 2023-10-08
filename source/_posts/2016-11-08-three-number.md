---
title: 找出有序数组中 3 个和为 0 的数
categories:
    - LeetCode
tags:
    - LeetCode
    - 学习笔记
    - C/C++
date: 2016-11-08 01:26:19
---

前几天偶尔看到一道题，感觉蛮有意思的，在自己思考，外加上网查询之后，找到了一个比较完美的算法解决。问题描述如下：

>  给定有序排列的N个整数，找出其中3个数相加和为0，输出所有的不重复的3个数，要求输出的结果依然有序。
>  在单行内，输出的顺序和原来一致，每行之间的顺序和第一个数字在原数列中的顺序一致（如果相同则向后依次比较）
>  INPUT :
>  5
>  -2 -1 0 1 2
>  OUTPUT:
>  -2 0 2
>  -1 0 1
>  INPUT :
>  5
>  2 2 0 -2 -4
>  OUTPUT:
>  2 2 -4
>  2 0 -2

<!-- more -->

# 思路

1. 首先思考：寻找有序数组（下文说的数组均是升序排列的）中两个和为 k 的值。很简单，两个指针 i 和 j 分别指向数组两端，计算两数的和，大于 k 则 `j--`，否则 `i++`，直到寻找到两个数和为 k 或者 `i > j` 时停止。

2. 上题中可以转换为寻找数组中两个和为 -k 的数，k 为数组中的元素，我们只需要再加一层循环，遍历数组中的值，作为 k 就可以了。

3. 那么如何解决结果中有重复值的情况呢？比如我们输入 [-4, -2, 0, 2, 2] 这5个数，会输出两次 [-2, 0, 2]。其实这个也很好判断，当我们确定了三个数中的一个数，那么后面求出的两个数就确定下来了，比如我们确定"第一个数"是 -5，那么可能求出后面两个数是 [2, 3] 和 [1, 4]，当我们循环到下一个"第一个数"的时候，如果还是 -5，那求出的数据肯定还是重复的，所以直接跳过就好。【这块的描述好像不是很清楚啊！(╯‵□′)╯︵┻━┻ 不明白的话看下面的代码肯定一下子就明白了呢！】

# 代码实现

```c++
#include <iostream>
#include <vector>

using namespace std;

vector<vector<int>> findThreeNumber(vector<int> arr, bool asc) {
	int size = arr.size();
	vector<vector<int>> result;	// 保存结果
	for (int i = 0; i < size; i++) {	// 保证"第一个数"在数组中
		int m = i + 1;
		int n = size - 1;
		while (m < n) {
			int sum = arr[m] + arr[n];	// 另外两个数的和
			if (sum + arr[i]  > 0) {
				asc ? n-- : m++;	// 升序和降序的操作不一样
			}
			else if (sum + arr[i] < 0) {
				asc ? m++ : n--;
			}
			else {	// 找到三数和为0
				vector<int> v(3);
				v[0] = arr[i];
				v[1] = arr[m];
				v[2] = arr[n];
				result.push_back(v);

				// "第二个数"和"第三个数"分别判断其下个数是否重复
				do {
					m++;
				} while (m < n && arr[m - 1] == arr[m]);

				do {
					n--;
				} while (m < n && arr[n + 1] == arr[n]);
			}
		}

		// 外层循环"第一个数"，如果下一个"第一个数"相同，则为重复数据
		while (i < size - 2 && arr[i + 1] == arr[i]) {
			i++;
		}
	}
	return result;
}

int main() {
	bool asc = true;	// 升序
	int n = 0;
	cin >> n;
	vector<int> arr(n);
	for (int i = 0; i < n; i++) {
		cin >> arr[i];
		if (i > 0 && arr[i] < arr[i - 1]) {
			asc = false;
		}
	}

	vector<vector<int>> result = findThreeNumber(arr, asc);
	for each (vector<int> v in result) {
		cout << v[0] << " " << v[1] << " " << v[2] << endl;
	}
	system("pause");
}
```

---

> 文章标题：<a href='{{ permalink }}' title='{{ title }}' >{{ title }}</a>
> 文章作者：[cylong](http://www.cylong.com/about/ "cylong")
> 文章链接：<a href='{{ permalink }}' title='{{ title }}' >{{ permalink }}</a>
> 有问题或者建议欢迎在下方评论。欢迎转载、引用，但希望标明出处，感激不尽(●'◡'●)
