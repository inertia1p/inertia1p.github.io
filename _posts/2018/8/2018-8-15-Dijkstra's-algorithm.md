---
layout:     post
title:      Dijkstra's algorithm
subtitle:   人的恐惧大部分时候来自未知而不是危险
date:       2018-08-15
author:     inertia1p
header-img: img/post-bg-unix-linux.jpg
keywords_post:  "PAT，C++，算法，图"
catalog: true
tags:
    - C++
    - PAT
    - 算法
    - 图
---

## Summary

Dijkstra's  algorithm was conceived by computer scientist Edsger W. Dijkstra in 1956 and published three years later(1959).  

![Dijkstra_Animation.gif](https://inertia1p.github.io\img_post\Dijkstra_Animation.gif)

>Dijkstra's algorithm to find the shortest path between a and b. It picks the unvisited vertex with the lowest distance, calculates the distance through it to each unvisited neighbor, and updates the neighbor's distance if smaller. Mark visited (set to red) when done with neighbors.   ——from wiki

## Algorithm

1. **Mark all nodes unvisited**. Create a set of all the unvisited nodes called the unvisited set.
2. Assign to every node **a tentative distance value**(inf): set it to zero for our initial node and to infinity for all other nodes. Set the initial node as current.
3. For the current node, **consider all of its unvisited neighbors** and calculate their tentative distances through the current node. **Compare the newly calculated tentative distance to the current assigned value and assign the smaller one**. For example, if the current node A is marked with a distance of 6, and the edge connecting it with a neighbor B has length 2, then the distance to B through A will be 6 + 2 = 8. If B was previously marked with a distance greater than 8 then change it to 8. Otherwise, keep the current value.
4. **When we are done considering all of the unvisited neighbors of the current node, mark the current node as visited and remove it from the unvisited set**. A visited node will never be checked again.
5. If the destination node has been marked visited (when planning a route between two specific nodes) or if the smallest tentative distance among the nodes in the unvisited set is infinity (when planning a complete traversal; occurs when there is no connection between the initial node and **remaining unvisited nodes**), **then stop**. The algorithm has finished.
6. Otherwise, select the unvisited node that is marked with the smallest tentative distance, set it as the new "current node", and go back to step 3.

## PAT Advanced Level 1003 Emergency

```c++
#include<iostream>
#include<algorithm>
using namespace std;
int n, m, c1, c2;
int e[510][510], weight[510], dis[510], num[510], w[510];
bool visit[510];
const int inf = 99999999;
int main()
{
	cin >> n >> m>>c1>>c2;
	for (int i = 0; i < n; i++) {
		scanf("%d", &weight[i]);
	}
	fill(e[0], e[0] + 510 * 510, inf);
	fill(dis, dis + 510, inf);
	for (int i = 0,a,b,c; i < m; i++) {
		scanf("%d%d%d", &a, &b, &c);
		e[a][b] = e[b][a] = c;
	}
	dis[c1] = 0;   /*设置原点*/
	w[c1] = weight[c1];  /*原点人数*/
	num[c1] = 1;  /*原点到原点一条线路*/
	for (int i = 0; i < n; i++) {
		int u = -1, min = inf;    
		for (int j = 0; j < n; j++) {   /*得到一个新的*最近的*未访问城市*/
			if (visit[j] == false && dis[j] < min) {  /*如果未访问且于旧原点可达*/  
				u = j;
				min = dis[j]; /*用作比较周边城市的远近*/
			}
		}
		if (u == -1) break;  
		visit[u] = true;  /*标记*已确定到达该城市的最短路径*/   
		for (int v = 0; v < n; v++) {  /*遍历所有城市来找到*/    
			if (visit[v] == false && e[u][v] != inf) { /*新城市可达*/
				if (dis[u] + e[u][v] < dis[v]) { /*比较当前确定路径和新路径大小*/    
					dis[v] = dis[u] + e[u][v];
					num[v] = num[u]; /*路径数目不变*/
					w[v] = w[u] + weight[v];
				}
				else if (dis[u] + e[u][v] == dis[v]) { /*长度相等*/
					num[v] = num[v] + num[u];   /*到达该城市方法就加上到达前一座城市的方法*/
					if (w[u] + weight[v] > w[v]) w[v] = w[u] + weight[v]; /*比较两种方法的最大人数*/
				}
			}
		}
	}
	printf("%d %d", num[c2], w[c2]);
	system("pause");
	return 0;
}
```

关键在确立bool visit[]和初始化路径inf，这样第一可以确定哪些点是判断原点到该点距离完毕的，第二inf是可以作不可达的比较，不然程序中难以评判不可达的标准。


## PAT Advanced Level 100Battel Over Cities

```c++
#include <cstdio>
#include<iostream>
#include <algorithm>
using namespace std;
int road[1010][1010];
bool visit[1010];
int N, M, K;
// dfs深度遍历，这里用于找出一个连通分图
void dfs(int t) {
	visit[t] = true;
	for (int i = 1; i <= N; i++) {
		if (visit[i] == false && road[t][i] == 1) {
			dfs(i);
		}
	}
}
int main()
{
	cin >> N >> M >> K;
	for (int i = 0; i < M; i++) {
		int a, b;
		scanf("%d %d", &a, &b);
		road[a][b] = road[b][a] = 1;
	}
	for (int i = 0; i < K; i++) {
		int chek,cnt=0;
		fill(visit, visit + 1010, false);
		cin >> chek;
		visit[chek] = true;
    //如果有一个连通分图cnt就加1
		for (int j = 1; j <= N; j++) {
			if (visit[j] == false) {
				dfs(j);
				cnt++;
			}
		}
		cout << cnt - 1 << endl;
	}
	system("pause");
	return 0;
}
```

这题的题意是一开始给你一张强连通图，然后他给你去掉一个点，然后问你至少要几条边才能把剩下的点连重新连成强连通图。这里可以想到，要把重新组成强连通图只需要把生成的连通分图重连，也就是需要cnt（连通分图数）-1条边。  
所以一个循环DFS算法，统计连通分图数就可以了。  
这个应该也算Dijkstra算法的衍生。  


## 震惊！！99%的人都做不出的题目PAT Advanced Level 1018 Public Bike Management

呜呜呜呜  @ A @

又发现自己有多菜。这题我一定要自己先想个方法出来！

更新。
真的菜，无法接受自己臃肿的算法，内存占太多了，学习了前辈的算法。Dijkstra和DFS。先用Dijkstra把最短路径找出来，使用pre[]来把每个节点的最小路径前置节点记录，用于DFS搜索时的条件，这样就可以重新遍历一遍最短路径，又占用比较小的内存。算法结构也清晰简单。我还看了几个另外的解题思路。觉得记录前置节点绝对是最简单的做法。

```c++
#include <iostream>
#include <algorithm>
#include <vector>
using namespace std;
const int inf = 99999999;
int cmax, n, sp, m;
int minNeed = inf, minBack = inf;
int e[510][510], dis[510], weight[510];
bool visit[510];
vector<int> pre[510];
vector<int> path, temppath;
void dfs(int v) {
	temppath.push_back(v);
	if (v == 0) {
		int need = 0, back = 0;
		for (int i = temppath.size() - 1; i >= 0; i--) {
			int id = temppath[i];
			if (weight[id] > 0) {
				back += weight[id];
			}
			else {
				if (back > (0 - weight[id])) {
					back += weight[id];
				}
				else {
					need += ((0 - weight[id]) - back);
					back = 0;
				}
			}
		}
		if (need < minNeed) {
			minNeed = need;
			minBack = back;
			path = temppath;
		}
		else if (need == minNeed && back < minBack) {
			minBack = back;
			path = temppath;
		}
		temppath.pop_back();
		return;
	}
	for (int i = 0; i < pre[v].size(); i++)
		dfs(pre[v][i]);
	temppath.pop_back();
}
int main() {
	fill(e[0], e[0] + 510 * 510, inf);
	fill(dis, dis + 510, inf);
	scanf("%d%d%d%d", &cmax, &n, &sp, &m);
	for (int i = 1; i <= n; i++) {
		scanf("%d", &weight[i]);
		weight[i] = weight[i] - cmax / 2;
	}
	for (int i = 0; i < m; i++) {
		int a, b;
		scanf("%d%d", &a, &b);
		scanf("%d", &e[a][b]);
		e[b][a] = e[a][b];
	}
	dis[0] = 0;
	for (int i = 0; i <= n; i++) {
		int u = -1, minn = inf;
		for (int j = 0; j <= n; j++) {
			if (visit[j] == false && dis[j] < minn) {
				u = j;
				minn = dis[j];
			}
		}
		if (u == -1) break;
		visit[u] = true;
		for (int v = 0; v <= n; v++) {
			if (visit[v] == false && e[u][v] != inf) {
				if (dis[v] > dis[u] + e[u][v]) {
					dis[v] = dis[u] + e[u][v];
					pre[v].clear();
					pre[v].push_back(u);
				}
				else if (dis[v] == dis[u] + e[u][v]) {
					pre[v].push_back(u);
				}
			}
		}
	}
	dfs(sp);
	printf("%d 0", minNeed);
	for (int i = path.size() - 2; i >= 0; i--)
		printf("->%d", path[i]);
	printf(" %d", minBack);
	return 0;
}
```

**PS：以后甲级题复习一定记得重新做！别再做不出来了，丢人！**


可能待续 。。。   。。。
