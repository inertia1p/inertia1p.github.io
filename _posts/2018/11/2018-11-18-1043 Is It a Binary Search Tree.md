---
layout:     post
title:      1043 Is It a Binary Search Tree（25 分）
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
1043 Is It a Binary Search Tree （25 分）
A Binary Search Tree (BST) is recursively defined as a binary tree which has the following properties:

The left subtree of a node contains only nodes with keys less than the node's key.
The right subtree of a node contains only nodes with keys greater than or equal to the node's key.
Both the left and right subtrees must also be binary search trees.
If we swap the left and right subtrees of every node, then the resulting tree is called the Mirror Image of a BST.

Now given a sequence of integer keys, you are supposed to tell if it is the preorder traversal sequence of a BST or the mirror image of a BST.

Input Specification:
Each input file contains one test case. For each case, the first line contains a positive integer N (≤1000). Then N integer keys are given in the next line. All the numbers in a line are separated by a space.

Output Specification:
For each test case, first print in a line YES if the sequence is the preorder traversal sequence of a BST or the mirror image of a BST, or NO if not. Then if the answer is YES, print in the next line the postorder traversal sequence of that tree. All the numbers in a line must be separated by a space, and there must be no extra space at the end of the line.

Sample Input 1:
```
7
8 6 5 7 10 8 11
```
Sample Output 1:
```
YES
5 7 6 8 11 10 8
```
Sample Input 2:
```
7
8 10 11 8 6 7 5
```
Sample Output 2:
```
YES
11 8 10 7 5 6 8
```
Sample Input 3:
```
7
8 6 8 5 10 9 11
```
Sample Output 3:
```
NO
```

**难点：BST的前序后序转换和前序判断BST**

## 思路

树的遍历首先想到的是递归结构。设BST根为root，BST的左子树根和右子树根分别应该是root+1和```while(i<=ed&&pre[i]<pre[root])i++```得到的pre[i]。但是这样判断出的右子树节点不能保证右子树中的节点都大于等于pre[root]。所以应该用```		while (i <= ed && pre[i] < pre[root]) i++;
		while (j > root && pre[j] >= pre[root]) j--;```来保证左右子树中节点的正确性。同时，如果是一颗BST树，左右子树应该包括了所有的子节点，所以可以用```if(i-j!=1) return;```来减少无用的递归。但是，之前的i,j已经保证了如果它不是一颗BST树，那么它就会有节点没有被遍历到。  
    所以```post.size()!=n```可用来判断树是否为BST。判断完一次后，需要在判断一次镜像BST的可能。

## 解题代码

```C++
#include<bits/stdc++.h>
using namespace std;
vector<int> pre, post;
int m = 1;
void judge(int root,int ed) {
	if (root > ed) return;
	int i = root+1;
	int j = ed;
	if (m) {
		while (i <= ed && pre[i] < pre[root]) i++;
		while (j > root && pre[j] >= pre[root]) j--;
	}
	else {
		while (i <= ed && pre[i] >= pre[root]) i++;
		while (j > root && pre[j] < pre[root]) j--;
	}
	judge(root + 1, j);
	judge(i,ed);
	post.push_back(pre[root]);
}


int main() {
	int n;
	cin >> n;
	pre.resize(n);
	for (int i = 0; i < n; i++) {
		scanf("%d", &pre[i]);
	}
	judge(0, n - 1);
	if (post.size() != n) {
		post.clear();
		m = 0;
		judge(0, n - 1);
	}
	if (post.size() == n) {
		printf("YES\n");
		for (int i = 0; i < post.size(); i++) {
			if (i != 0) printf(" ");
			printf("%d", post[i]);
		}
	}
	else {
			printf("NO");
	}
	return 0;
}
```
