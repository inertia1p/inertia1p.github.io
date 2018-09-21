---
layout:     post
title:      树的前序中序后序转换
subtitle:   积少成多不要心急，沉心
date:       2018-09-14
author:     inertia1p
header-img: img/post-bg-github-cup.jpg
keywords_post:  "binary tree 数据结构"
catalog: true
tags:
    - C++
    - 树
    - 数据结构
---

## 引子 PAT1043 Is It a Binary Search Tree (25)

1043. Is It a Binary Search Tree (25)-PAT甲级真题
A Binary Search Tree (BST) is recursively defined as a binary tree which has the following properties:

The left subtree of a node contains only nodes with keys less than the node’s key.
The right subtree of a node contains only nodes with keys greater than or equal to the node’s key.
Both the left and right subtrees must also be binary search trees.
If we swap the left and right subtrees of every node, then the resulting tree is called the Mirror Image of a BST.

Now given a sequence of integer keys, you are supposed to tell if it is the preorder traversal sequence of a BST or the mirror image of a BST.

Input Specification:

Each input file contains one test case. For each case, the first line contains a positive integer N (<=1000). Then N integer keys are given in the next line. All the numbers in a line are separated by a space.

Output Specification:

For each test case, first print in a line “YES” if the sequence is the preorder traversal sequence of a BST or the mirror image of a BST, or “NO” if not. Then if the answer is “YES”, print in the next line the postorder traversal sequence of that tree. All the numbers in a line must be separated by a space, and there must be no extra space at the end of the line.

>Sample Input 1:
7
8 6 5 7 10 8 11

>Sample Output 1:
YES
5 7 6 8 11 10 8

>Sample Input 2:
7
8 10 11 8 6 7 5

>Sample Output 2:
YES
11 8 10 7 5 6 8

>Sample Input 3:
7
8 6 8 5 10 9 11

>Sample Output 3:
NO

题意是判断是否是一个二叉搜索树的前序遍历。是就输出他的后序遍历。

## 样例代码

```C++
#include <cstdio>
#include <vector>
using namespace std;
bool isMirror;
vector<int> pre, post;
void getpost(int root, int tail) {
    if(root > tail) return ;
    int i = root + 1, j = tail;
    if(!isMirror) {
        while(i <= tail && pre[root] > pre[i]) i++;
        while(j > root && pre[root] <= pre[j]) j--;
    } else {
        while(i <= tail && pre[root] <= pre[i]) i++;
        while(j > root && pre[root] > pre[j]) j--;
    }
    getpost(root + 1, j);
    getpost(i, tail);
    post.push_back(pre[root]);
}
int main() {
    int n;
    scanf("%d", &n);
    pre.resize(n);
    for(int i = 0; i < n; i++)
        scanf("%d", &pre[i]);
    getpost(0, n - 1);
    if(post.size() != n) {
        isMirror = true;
        post.clear();
        getpost(0, n - 1);
    }
    if(post.size() == n) {
        printf("YES\n%d", post[0]);
        for(int i = 1; i < n; i++)
            printf(" %d", post[i]);
    } else {
        printf("NO");
    }
    return 0;
}
```

关键在取得后序遍历的算法。我对这种树递归写法没有系统规范的了解。  
1. ```if(root > tail) return ;```这一句是必须的，用来判断递归的结束。
2. 前序，中序，后序输出区别在于把输出语句放在左右子树深度遍历语句的什么位置。
3. 每次递归的主要问题就是确定左右子树范围和子树的根。

## 已知后序与中序输出前序

```C++
#include <cstdio>
using namespace std;
int post[] = {3, 4, 2, 6, 5, 1};
int in[] = {3, 2, 4, 1, 6, 5};
void pre(int root, int start, int end) {
    if(start > end) return ;
    int i = start;
    while(i < end && in[i] != post[root]) i++;
    printf("%d ", post[root]);
    pre(root - 1 - end + i, start, i - 1);
    pre(root - 1, i + 1, end);
}

int main() {
    pre(5, 0, 5);
    return 0;
}
```
因为在中序中，root的左边所有的数就root的左子树，root右边的所有数是root的右子树。且后序中的最后一个数为树的根节点。所以找post[end]==in[i]，i下标的左边{3,2,4}就是左子树，i下标的右边{6，5}就是右子树。  
所以，post[end]往前数2，是右子树组成，再往前数3是左子树组成。  递归算法完成。

## 已知前序与中序输出后序

```C++
#include <cstdio>
using namespace std;
int pre[] = {1, 2, 3, 4, 5, 6};
int in[] = {3, 2, 4, 1, 6, 5};
void post(int root, int start, int end) {
    if(start > end) return ;
    int i = start;
    while(i <= end && in[i] != pre[root]) i++;  
    post(root+1, start, i-1);
    post(root+1+i, i+1 , end);
    printf("%d ", post[root]);
}

int main() {
    post(0, 0, 5);
    return 0;
}
```

## 1020 Tree Traversals （25 分）

1020 Tree Traversals （25 分）
Suppose that all the keys in a binary tree are distinct positive integers. Given the postorder and inorder traversal sequences, you are supposed to output the level order traversal sequence of the corresponding binary tree.

Input Specification:
Each input file contains one test case. For each case, the first line gives a positive integer N (≤30), the total number of nodes in the binary tree. The second line gives the postorder sequence and the third line gives the inorder sequence. All the numbers in a line are separated by a space.

Output Specification:
For each test case, print in one line the level order traversal sequence of the corresponding binary tree. All the numbers in a line must be separated by exactly one space, and there must be no extra space at the end of the line.

>Sample Input:
7
2 3 1 5 7 6 4
1 2 3 4 5 6 7

>Sample Output:
4 1 6 3 5 7 2

```c++
#include <iostream>
#include <vector>
#include<map>
using namespace std;
int N;
vector<int> post,in;
map<int, vector<int>> m;
void level(int root,int start,int end,int l) {
	if (start > end) return;
	int i = start;
	while (in[i] != post[root] && i < end) i++;
	m[l++].push_back(post[root]);
	level(root-1-(end-i),start,i-1,l);
	level(root - 1, i + 1, end, l);
}
int main() {
	int N;
	cin >> N;
	for (int i = 0; i < N; i++) {
		int t;
		cin >> t;
		post.push_back(t);
	}
	for (int i = 0; i < N; i++) {
		int t;
		cin >> t;
		in.push_back(t);
	}
	level(N-1,0,N-1,0);
	for (map<int, vector<int>> ::iterator it = m.begin(); it != m.end(); it++) {
		for (int i = 0; i < it->second.size(); i++) {
			if (it != m.begin()) cout << ' ';
			cout << it->second[i];
		}
	}
	system("pause");
	return 0;
}
```

这一题其实就是后序，中序转前序的应用。如果理解记忆了```    pre(root - 1 - end + i, start, i - 1);
    pre(root - 1, i + 1, end);```这一题就是水题。因为需要思考的就是在遍历过程中记录当前层的数据。我这里用的是map和l。在一层遍历中l++即可。
