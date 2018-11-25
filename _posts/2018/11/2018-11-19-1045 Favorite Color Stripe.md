---
layout:     post
title:      1045 Favorite Color Stripe
subtitle:   PAT重要题整理
date:       2018-11-19
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

1045 Favorite Color Stripe （30 分）
Eva is trying to make her own color stripe out of a given one. She would like to keep only her favorite colors in her favorite order by cutting off those unwanted pieces and sewing the remaining parts together to form her favorite color stripe.

It is said that a normal human eye can distinguish about less than 200 different colors, so Eva's favorite colors are limited. However the original stripe could be very long, and Eva would like to have the remaining favorite stripe with the maximum length. So she needs your help to find her the best result.

Note that the solution might not be unique, but you only have to tell her the maximum length. For example, given a stripe of colors {2 2 4 1 5 5 6 3 1 1 5 6}. If Eva's favorite colors are given in her favorite order as {2 3 1 5 6}, then she has 4 possible best solutions {2 2 1 1 1 5 6}, {2 2 1 5 5 5 6}, {2 2 1 5 5 6 6}, and {2 2 3 1 1 5 6}.

Input Specification:
Each input file contains one test case. For each case, the first line contains a positive integer N (≤200) which is the total number of colors involved (and hence the colors are numbered from 1 to N). Then the next line starts with a positive integer M (≤200) followed by M Eva's favorite color numbers given in her favorite order. Finally the third line starts with a positive integer L (≤10
​4
​​ ) which is the length of the given stripe, followed by L colors on the stripe. All the numbers in a line a separated by a space.

Output Specification:
For each test case, simply print in a line the maximum length of Eva's favorite stripe.

Sample Input:
```
6
5 2 3 1 5 6
12 2 2 4 1 5 5 6 3 1 1 5 6
```
Sample Output:
```
7
```

**难点：动态规划**

## 思路

每个数从该位置往前找数，如果找到他的最大前面的数，就把那个数的cnt数加一赋给自己。最大长度是最后一个数的cnt值

## 解题代码

```C++
#include<bits/stdc++.h>
using namespace std;
int main() {
	int n,m,k;
	cin >> n>>m;
	vector<int> like(m);
	for (int i = 0; i < m; i++)	scanf("%d", &like[i]);
	cin >> k;
	vector<int> str(k);
	vector<int> cnt(k);
	for (int i = 0; i < k; i++) {
		scanf("%d", &str[i]);
		int f = 0;
		while (f<m&&str[i] != like[f]) f++;
		if (f == m)continue;
		int j = i-1;
		while(j>=0){
			if (find(like.begin(), like.begin() + f+1, str[j]) != like.begin() + f+1) {
				cnt[i] = cnt[j] + 1;
				break;
			}
			j--;
		}
	}
	cout<<cnt[k-1]+1;
	return 0;
}
```


**优化版：**

时间从40ms+ 提升到5ms左右。主要使用了BOOK来记录颜色order，这样就可以不用find来找order里的值，str中存的是顺序值。只要找到第一个顺序比尾值小的值即可。

```C++
#include<bits/stdc++.h>
using namespace std;
int book[201];
int main() {
	int n, m, k,t,x,num=0;
	scanf("%d %d", &n,&m);
	for (int i = 1; i <= m; i++) {
		scanf("%d", &x);
		book[x] = i;
	}
	scanf("%d", &k);
	vector<int> str(k),cnt(k);
	for (int i = 0; i < k; i++) {
		scanf("%d", &x);
		if (book[x] >= 1) str[num++]=book[x];
		int j = num-2 ;
		while (j >= 0) {
			if (str[j]<=str[num - 1]) {
				cnt[num - 1] = cnt[j] + 1;
				break;
			}
			j--;
		}
	}
	cout << cnt[num-1] + 1;
	return 0;
}
```
