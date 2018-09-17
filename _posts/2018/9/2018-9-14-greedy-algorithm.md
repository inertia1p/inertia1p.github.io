---
layout:     post
title:      Greedy algorithm
subtitle:   持之以恒最为重
date:       2018-09-14
author:     inertia1p
header-img: img/post-greedy.jpg
keywords_post:  "PAT，C++，贪心，算法"
catalog: true
tags:
    - C++
    - PAT
    - 贪心算法
---

## Summary

In general, greedy algorithms have five components:

1. A candidate set, from which a solution is created
2. A selection function, which chooses the best candidate to be added to the solution
3. A feasibility function, that is used to determine if a candidate can be used to contribute to a solution
4. An objective function, which assigns a value to a solution, or a partial solution, and
5. A solution function, which will indicate when we have discovered a complete solution

Greedy algorithms produce good solutions on some mathematical problems, but not on others. Most problems for which they work will have two properties:

**Greedy choice property** 贪婪选择性
We can make whatever choice seems best at the moment and then solve the subproblems that arise later. The choice made by a greedy algorithm may depend on choices made so far, but not on future choices or all the solutions to the subproblem. It iteratively makes one greedy choice after another, reducing each given problem into a smaller one. In other words, a greedy algorithm never reconsiders its choices. This is the main difference from dynamic programming, which is exhaustive and is guaranteed to find the solution. After every stage, dynamic programming makes decisions based on all the decisions made in the previous stage, and may reconsider the previous stage's algorithmic path to solution.
**Optimal substructure** 满足最优子结构
"A problem exhibits optimal substructure if an optimal solution to the problem contains optimal solutions to the sub-problems."


问题：判断问题可分子结构？最优子结构？

如：矩形地图中填充格子格子中填数字作为到此距离，从左下到右上，那么最优标准就是路线中格子数字总和最小的路径。可以感受到对于最优路径上的每一个点，从起点到这个点的最优路径依旧是这一条路。


## 1038 Recover the Smallest Number （30 分）

#### 问题描述
1038 Recover the Smallest Number （30 分）
Given a collection of number segments, you are supposed to recover the smallest number from them. For example, given { 32, 321, 3214, 0229, 87 }, we can recover many numbers such like 32-321-3214-0229-87 or 0229-32-87-321-3214 with respect to different orders of combinations of these segments, and the smallest number is 0229-321-3214-32-87.

Input Specification:
Each input file contains one test case. Each case gives a positive integer N (≤10
​4) followed by N number segments. Each segment contains a non-negative integer of no more than 8 digits. All the numbers in a line are separated by a space.

Output Specification:
For each test case, print the smallest number in one line. Notice that the first digit must not be zero.

Sample Input:
5 32 321 3214 0229 87
Sample Output:
22932132143287

#### 我的代码

```c++

// PATtest.cpp: 定义控制台应用程序的入口点。
//

#include "stdafx.h"
#include <iostream>
#include <vector>
#include<string>
#include<algorithm>
#include<map>
using namespace std;
vector<string> v;
map<char, int> head;
int cmp1(string a, string b) {
	return a < b;
}
bool cmp(string a, string b) {
	int i;
	for (i = 0; i < a.length() && i < b.length(); i++) {
		if (a[i] > b[i]) {
			head[b[0]]--;
			return false;
		}
		if (a[i] < b[i]) {
			head[a[0]]--;
			return true;
		}
	}
	if (i == a.length()) {
		for (map<char, int>::iterator it = head.begin(); it != head.end(); it++) {
			if (it->second>0 && it->first < b[i]) {
				return true;
			}
		}
		return false;
	}
	else {
		for (map<char, int>::iterator it = head.begin(); it != head.end(); it++) {
			if (it->second>0&&it->first < a[i]) {
				return false;
			}
		}
		return true;
	}
}

int main()
{
	int N;
	cin >> N;
	for (int i = 0; i < N; i++) {
		string t;
		cin >> t;
		v.push_back(t);
		head[t[0]]++ ;
	}
	sort(v.begin(), v.end(),cmp1);
	sort(v.begin(), v.end(),cmp);
	for (int i = 0; i < N; i++) {
		if (i == 0) {
			cout << stoi(v[i]);
		}
		else {
			cout << v[i];
		}
	}
	system("pause");
	return 0;
}
```

这个是我最初的想法，用代码实现一个正常的思路。比较两个string哪一个在前，首先要在string大小的定义上是满足的。然后在无法判断基础上，使用map记录所有string 的首字母，如果在所有没有用到的首字母中，两个string中较长的string后一位的字母是总小于所有首字母的情况下，就应该取这一个string。否则，就取短的一个string。  
但是这真的远远不是优秀的解法。我在做出这一题的同时也对贪心算法的思维方式有深深的疑惑和向往。来看看代码吧！

#### 样例代码

```c++
#include <iostream>
#include <string>
#include <algorithm>
using namespace std;
bool cmp0(string a, string b) {
    return a + b < b + a;
}
string str[10010];
int main() {
    int n;
    scanf("%d", &n);
    for(int i = 0; i < n; i++)
        cin >> str[i];
    sort(str, str + n, cmp0);
    string s;
    for(int i = 0; i < n; i++)
        s += str[i];
    while(s.length() != 0 && s[0] == '0')
        s.erase(s.begin());
    if(s.length() == 0) cout << 0;
    cout << s;
    return 0;
}
```


在长度上，算法的简洁性也有直观的感受。再仔细看看它是怎么做到的。  
贪心算法，是寻找的局部最优解。同时问题满足局部最优解同时也是整体部分的最优解。这一个问题其实我在思考的时候应该注意到，在0229-321-3214-32-87这个最优解中，基础排序无法判断的321-3214-32满足321-3214<3214-321/3214-32<32-3214这个地方判断它们排序的关键在在相邻string的组合中取较小的那一个。  
整个问题提出要找到string组合中最小情况的组合，那么在思考的时候就应该考虑一下string组合再判断是否是符合题意的。另外，我有点小感受是：在考虑贪心问题的时候，觉得这个问题可能有贪心做法的时候，应该举出较小的案例，思考解决方法。  
如果这一题我去思考{321,3214}的排序方法（虽然还是可能去想比较3和1）就会容易发现这只是两种可能性的抉择（321-3214/3214-321），显而易见。  
