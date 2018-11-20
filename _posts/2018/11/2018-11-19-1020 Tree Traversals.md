---
layout:     post
title:      1020 Tree Traversals （25 分）
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

1020 Tree Traversals （25 分）
Suppose that all the keys in a binary tree are distinct positive integers. Given the postorder and inorder traversal sequences, you are supposed to output the level order traversal sequence of the corresponding binary tree.

Input Specification:
Each input file contains one test case. For each case, the first line gives a positive integer N (≤30), the total number of nodes in the binary tree. The second line gives the postorder sequence and the third line gives the inorder sequence. All the numbers in a line are separated by a space.

Output Specification:
For each test case, print in one line the level order traversal sequence of the corresponding binary tree. All the numbers in a line must be separated by exactly one space, and there must be no extra space at the end of the line.

Sample Input:
```
7
2 3 1 5 7 6 4
1 2 3 4 5 6 7
```
Sample Output:
```
4 1 6 3 5 7 2
```
**难点：树的后序中序转层序**

## 思路

1. 通过后序中序来找到树的左右子树，递归找到所有的节点。
2. 往递归算法中传入L作为层序标志，按层序加入map。

## 解题代码

```C++
#include<bits/stdc++.h>
using namespace std;
vector<int> in, post;
map<int, vector<int>> m;

void lev(int root, int st, int ed, int l) {
	if (st > ed) return;
	int i = st;
	while (i <= ed && in[i] != post[root])i++;
	m[l++].push_back(post[root]);
	lev(root-1-(ed-i), st,i - 1, l);
	lev(root - 1, i + 1, ed, l);
}

int main() {
	int n;
	cin >> n;
	for (int i = 0; i < n; i++) {
		int t;
		cin >> t;
		post.push_back(t);
	}
	for (int i = 0; i < n; i++) {
		int t;
		cin >> t;
		in.push_back(t);
	}
	lev(n - 1, 0, n - 1, 0);
	for (map<int, vector<int>> ::iterator it = m.begin(); it != m.end(); it++) {
		for (int i = 0; i < it->second.size(); i++) {
			if (it != m.begin()) cout << ' ';
			cout << it->second[i];
		}
	}
	return 0;
}
```
