---
layout:     post
title:      1007 Maximum Subsequence Sum （25 分）动态规划
subtitle:   PAT重要题整理
date:       2018-11-18
author:     inertia1p
header-img: img/post-bg-github-cup.jpg
keywords_post:  "PAT重要题整理，PAT"
catalog: true
tags:
    - C++
    - PAT重要题整理
    - PAT
---

## 题目

1007 Maximum Subsequence Sum （25 分）
Given a sequence of K integers { N
​1
​​ , N
​2
​​ , ..., N
​K
​​  }. A continuous subsequence is defined to be { N
​i
​​ , N
​i+1
​​ , ..., N
​j
​​  } where 1≤i≤j≤K. The Maximum Subsequence is the continuous subsequence which has the largest sum of its elements. For example, given sequence { -2, 11, -4, 13, -5, -2 }, its maximum subsequence is { 11, -4, 13 } with the largest sum being 20.

Now you are supposed to find the largest sum, together with the first and the last numbers of the maximum subsequence.

Input Specification:
Each input file contains one test case. Each case occupies two lines. The first line contains a positive integer K (≤10000). The second line contains K numbers, separated by a space.

Output Specification:
For each test case, output in one line the largest sum, together with the first and the last numbers of the maximum subsequence. The numbers must be separated by one space, but there must be no extra space at the end of a line. In case that the maximum subsequence is not unique, output the one with the smallest indices i and j (as shown by the sample case). If all the K numbers are negative, then its maximum sum is defined to be 0, and you are supposed to output the first and the last numbers of the whole sequence.

Sample Input:
```
10
-10 1 2 3 4 -5 -23 3 7 -21
```
Sample Output:
```
10 1 4
```

**难点：动态规划**

## 思路

1. 设立一个_max记录最大子序列的和，同时用ans_st,ans_ed，记录下答案首下标和尾下标。
2. 设两个临时下标作移动的首尾下标（t_st,t_ed）。当临时尾下标读取下一个数时，子序列和为负数时。修改首下标为t_ed。这是因为，如果把每个子序列的和作为一个元素。那么我们要得到一个最大和就不会把一个负数加入子序列。
3. 题目要求如果序列中全是负数，那么max=0，输出首数和尾数。


## 解题代码

```C++
#include<bits/stdc++.h>
using namespace std;

int main() {
	int n, ans_st, ans_ed, t_st=0, t_ed=0, Max=-1, t_sum=0,flag=0;
	cin >> n;
	vector<int> v(n);
	for (int i = 0; i < n; i++) {
		scanf("%d", &v[i]);
		if (v[i] >= 0) flag = 1;
	}
	if (flag == 0) {
		cout << 0 << " " << v[0]<< ' ' << v[n-1];
		return 0;
	}
	while (t_ed < n) {
		t_sum += v[t_ed];
		if (t_sum > Max) {
			Max = t_sum;
			ans_st = t_st;
			ans_ed = t_ed;
		}
		t_ed++;
		if (t_sum < 0) {
			t_st = t_ed;
			t_sum = 0;
		}
	}
	cout << Max << " " << v[ans_st] << ' ' << v[ans_ed];
	return 0;
}

```
