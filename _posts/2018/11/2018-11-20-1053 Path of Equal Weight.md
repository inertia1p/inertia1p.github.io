---
layout:     post
title:      1053 Path of Equal Weight （30 分）
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

1053 Path of Equal Weight （30 分）
Given a non-empty tree with root R, and with weight W
​i
​​  assigned to each tree node T
​i
​​ . The weight of a path from R to L is defined to be the sum of the weights of all the nodes along the path from R to any leaf node L.

Now given any weighted tree, you are supposed to find all the paths with their weights equal to a given number. For example, let's consider the tree showed in the following figure: for each node, the upper number is the node ID which is a two-digit number, and the lower number is the weight of that node. Suppose that the given number is 24, then there exists 4 different paths which have the same given weight: {10 5 2 7}, {10 4 10}, {10 3 3 6 2} and {10 3 3 6 2}, which correspond to the red edges in the figure.



Input Specification:
Each input file contains one test case. Each case starts with a line containing 0<N≤100, the number of nodes in a tree, M (<N), the number of non-leaf nodes, and 0<S<2
​30
​​ , the given weight number. The next line contains N positive numbers where W
​i
​​  (<1000) corresponds to the tree node T
​i
​​ . Then M lines follow, each in the format:

ID K ID[1] ID[2] ... ID[K]
where ID is a two-digit number representing a given non-leaf node, K is the number of its children, followed by a sequence of two-digit ID's of its children. For the sake of simplicity, let us fix the root ID to be 00.

Output Specification:
For each test case, print all the paths with weight S in non-increasing order. Each path occupies a line with printed weights from the root to the leaf in order. All the numbers must be separated by a space with no extra space at the end of the line.

Note: sequence {A
​1
​​ ,A
​2
​​ ,⋯,A
​n
​​ } is said to be greater than sequence {B
​1
​​ ,B
​2
​​ ,⋯,B
​m
​​ } if there exists 1≤k<min{n,m} such that A
​i
​​ =B
​i
​​ for i=1,⋯,k, and A
​k+1
​​ >B
​k+1
​​ .

Sample Input:
```
20 9 24
10 2 4 3 5 10 2 18 9 7 2 2 1 3 12 1 8 6 2 2
00 4 01 02 03 04
02 1 05
04 2 06 07
03 3 11 12 13
06 1 09
07 2 08 10
16 1 15
13 3 14 16 17
17 2 18 19
```
Sample Output:
```
10 5 2 7
10 4 10
10 3 3 6 2
10 3 3 6 2
```

**难点：树结构建立和dfs 基础练习**

## 思路

dfs，设vector<int> ans 记录路径值。当遍历到叶子判断完pop尾值。当sum==k&&当前节点是叶子节点，就输出ans。

## 解题代码

```C++
#include<bits/stdc++.h>
using namespace std;
typedef struct Node {
	int n, val;
	vector<int> child;
};
int n, m, k;
vector<Node> node;
vector<int> ans;
int notLeef[1001];
void dfs(int x,int sum) {
	sum += node[x].val;
	ans.push_back(node[x].val);
	if (sum == k&&notLeef[x]==0) {
		for (int i = 0; i < ans.size(); i++) {
			if (i != 0) printf(" ");
			printf("%d", ans[i]);
		}
		printf("\n");
	}
	for (int i = 0; i < node[x].child.size(); i++) {
		dfs(node[x].child[i], sum);
	}
	ans.pop_back();
}
bool cmp1(int a, int b) {
	return node[a].val > node[b].val;
}
int main() {
	int t,cs,tv;
	cin >> n >> m >> k;
	node.resize(n);
	for (int i = 0; i < n; i++) {
		scanf("%d", &t);
		node[i].val = t;
	}
	for (int i = 0; i < m; i++) {
		scanf("%d %d", &t,&cs);
		notLeef[t] = 1;
		for (int j = 0; j < cs; j++) {
			scanf("%d", &tv);
			node[t].child.push_back(tv);
		}
		sort(node[t].child.begin(), node[t].child.end(), cmp1);
	}
	dfs(0,0);
	return 0;
}

```
