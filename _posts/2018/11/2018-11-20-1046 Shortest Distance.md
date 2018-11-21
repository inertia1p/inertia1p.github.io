---
layout:     post
title:      1046 Shortest Distance （20 分）
subtitle:   PAT重要题整理
date:       2018-11-20
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

1046 Shortest Distance （20 分）
The task is really simple: given N exits on a highway which forms a simple cycle, you are supposed to tell the shortest distance between any pair of exits.

Input Specification:
Each input file contains one test case. For each case, the first line contains an integer N (in [3,10
​5
​​ ]), followed by N integer distances D
​1
​​  D
​2
​​  ⋯ D
​N
​​ , where D
​i
​​  is the distance between the i-th and the (i+1)-st exits, and D
​N
​​ is between the N-th and the 1st exits. All the numbers in a line are separated by a space. The second line gives a positive integer M (≤10
​4
​​ ), with M lines follow, each contains a pair of exit numbers, provided that the exits are numbered from 1 to N. It is guaranteed that the total round trip distance is no more than 10
​7
​​ .

Output Specification:
For each test case, print your results in M lines, each contains the shortest distance between the corresponding given pair of exits.

Sample Input:
```
5 1 2 4 14 9
3
1 3
2 5
4 1
```
Sample Output:
```
3
10
7
```
**难点: 降低时间复杂度**

## 思路

题目是给了一个环路要求给出两节点间的最短路径大小。也就是从顺时针和逆时针找最短的走法。如果只是把去下一跳的长度存到节点里的话，在计算大小的时候就需要再循环求和。但是我们可以在输入的时候把末尾节点作为初识节点。在每一个节点中存相对初识节点的距离。那么对于任两点x,y(x<y)的距离可用v[y-1]-v[x-1]表示。

## 解题代码

```C++
#include<bits/stdc++.h>
using namespace std;

int main() {
	int n,m,sum=0,t;
	cin >> n;
	vector<int> v(n+1);
	for (int i = 1; i <= n; i++) {
		scanf("%d", &t);
		sum += t;
		v[i] = sum;
	}
	cin >> m;
	for (int i = 0; i < m; i++) {
		int a, b;
		scanf("%d %d", &a, &b);
		if (a> b)
			swap(a, b);
		int temp = v[b - 1] - v[a - 1];
		printf("%d\n", min(sum-temp, temp));
	}
	return 0;
}
```

```C++
//超时代码
#include<bits/stdc++.h>
using namespace std;

int main() {
	int n,m,sum=0;
	cin >> n;
	vector<int> v(n+1);
	for (int i = 1; i <= n; i++) {
		scanf("%d", &v[i]);
		sum += v[i];
	}
	cin >> m;
	for (int i = 0; i < m; i++) {
		int a, b;
		scanf("%d %d", &a, &b);
		int ed = max(a, b);
		int st = min(a, b);
		int r1 = 0, r2 = 0;
		for (int j = st; j < ed; j++) {
			r1 += v[j];
		}
		r2 = sum - r1;
		printf("%d\n", min(r1, r2));
	}
	return 0;
}
```
