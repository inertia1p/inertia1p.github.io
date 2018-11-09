---
layout:     post
title:      并查集
subtitle:   做不来才开心，又有新知识学
date:       2018-09-14
author:     inertia1p
header-img: img/post-bg-github-cup.jpg
keywords_post:  "并查集，Disjoint-set"
catalog: true
tags:
    - C++
    - PAT
    - 并查集
    - Disjoint-set
---

## Summary

并查集，在一些有N个元素的集合应用问题中，我们通常是在开始时让每个元素构成一个单元素的集合，然后按一定顺序将属于同一组的元素所在的集合合并，其间要反复查找一个元素在哪个集合中。这一类问题近几年来反复出现在信息学的国际国内赛题中，其特点是看似并不复杂，但数据量极大，若用正常的数据结构来描述的话，往往在空间上过大，计算机无法承受；即使在空间上勉强通过，运行的时间复杂度也极高，根本就不可能在比赛规定的运行时间（1～3秒）内计算出试题需要的结果，只能用并查集来描述。
并查集是一种 **树型的数据结构**，用于处理一些不相交集合（Disjoint Sets）的合并及查询问题。常常在使用中以 **森林** 来表示。  
A disjoint-set forest consists of a number of elements each of which stores an id, a parent pointer, and, in efficient algorithms, either a size or a "rank" value.  

The parent pointers of elements are arranged to form one or more trees, each representing a set. If an element's parent pointer points to no other element, then the element is the root of a tree and is the representative member of its set. A set may consist of only a single element. However, if the element has a parent, the element is part of whatever set is identified by following the chain of parents upwards until a representative element (one without a parent) is reached at the root of the tree.  

Forests can be represented compactly in memory as arrays in which parents are indicated by their array index.  


## 1114 Family Property （25 分）


```c++
#include<bits/stdc++.h>
using namespace std;
struct DATA {
	int id, f, m, set, area;
	int c[10];
}dat[1005];
struct node {
	int id,p;
	double area,num;
	bool f;
}ans[10000];
int father[10005];
bool visit[10005];
//返回x所属集合的根节点
//如果x暂不属于任何集合的话，他自己成为新树的根节点
int find(int x) {
	while (x != father[x])
		x = father[x];
	return x;
}
//首先前提是保证了a，b是属于同集合的
//找到他们代表的集合id（root节点）
//然后因为题意要求返回ID最小的人代表集合
//所以把较小的集合id作为根连接上
void Union(int a, int b) {
	int fA = find(a);
	int fB = find(b);
	if (fA > fB)
		father[fA] = fB;
	else if (fA < fB)
		father[fB] = fA;
}
bool cmp(node a,node b){
	if (a.area != b.area) return a.area>b.area;
	else return a.id < b.id;
}
int main() {
	int n, k, cnt = 0;
	cin >> n;
	for (int i = 0; i < 10000; i++) {
		father[i] = i;
	}
	for (int i = 0; i < n; i++) {
		cin >> dat[i].id>>dat[i].f>>dat[i].m>>k;
		visit[dat[i].id] = true;
		if (dat[i].f != -1) {
			visit[dat[i].f] = true;
			Union(dat[i].f, dat[i].id);
		}
		if (dat[i].m != -1) {
			visit[dat[i].m] = true;
			Union(dat[i].m, dat[i].id);
		}
		for (int j = 0; j < k; j++) {
			cin >> dat[i].c[j];
			visit[dat[i].c[j]] = true;
			Union(dat[i].c[j], dat[i].id);
		}
		cin >> dat[i].set >> dat[i].area;
	}
	for (int i = 0; i < n; i++) {
		int id = find(dat[i].id);
		ans[id].id = id;
		ans[id].num += dat[i].set;
		ans[id].area += dat[i].area;
		ans[id].f = true;
	}
	for (int i = 0; i < 10000; i++) {
		if (visit[i] == true) {
			ans[find(i)].p++;
		}
		if (ans[i].f) cnt++;
	}
	for (int i = 0; i < 10000; i++) {
		if (ans[i].f) {
			ans[i].num = (double)(ans[i].num / ans[i].p);
			ans[i].area = (double)(ans[i].area / ans[i].p);
		}
	}
	sort(ans, ans + 10000, cmp);
	printf("%d\n", cnt);
	for (int i = 0; i < cnt; i++) {
		printf("%04d %d %.3f %.3f\n", ans[i].id, ans[i].p, ans[i].num, ans[i].area);
	}
	return 0;
}

```
