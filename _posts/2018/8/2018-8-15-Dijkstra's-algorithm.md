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

1. *Mark all nodes unvisited*. Create a set of all the unvisited nodes called the unvisited set.
2. Assign to every node *a tentative distance value*(inf): set it to zero for our initial node and to infinity for all other nodes. Set the initial node as current.
3. For the current node, *consider all of its unvisited neighbors* and calculate their tentative distances through the current node. *Compare the newly calculated tentative distance to the current assigned value and assign the smaller one*. For example, if the current node A is marked with a distance of 6, and the edge connecting it with a neighbor B has length 2, then the distance to B through A will be 6 + 2 = 8. If B was previously marked with a distance greater than 8 then change it to 8. Otherwise, keep the current value.
4. *When we are done considering all of the unvisited neighbors of the current node, mark the current node as visited and remove it from the unvisited set*. A visited node will never be checked again.
5. If the destination node has been marked visited (when planning a route between two specific nodes) or if the smallest tentative distance among the nodes in the unvisited set is infinity (when planning a complete traversal; occurs when there is no connection between the initial node and *remaining unvisited nodes*), *then stop*. The algorithm has finished.
6. Otherwise, select the unvisited node that is marked with the smallest tentative distance, set it as the new "current node", and go back to step 3.

## 应用题——PAT Advanced Level 1003 Emergency

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
	dis[c1] = 0;   //设置原点
	w[c1] = weight[c1];  //原点人数
	num[c1] = 1;  //原点到原点一条线路
	for (int i = 0; i < n; i++) {
		int u = -1, min = inf;    
		for (int j = 0; j < n; j++) {   //得到一个新的*最近的*未访问城市  
			if (visit[j] == false && dis[j] < min) {  //如果未访问且于旧原点可达  
				u = j;
				min = dis[j]; //用作比较周边城市的远近    
			}
		}
		if (u == -1) break;  
		visit[u] = true; // 标记*已确定到达该城市的最短路径   
		for (int v = 0; v < n; v++) {  //遍历所有城市来找到     
			if (visit[v] == false && e[u][v] != inf) { //新城市可达     
				if (dis[u] + e[u][v] < dis[v]) { //比较当前确定路径和新路径大小     
					dis[v] = dis[u] + e[u][v];
					num[v] = num[u]; //路径数目不变    
					w[v] = w[u] + weight[v];
				}
				else if (dis[u] + e[u][v] == dis[v]) { //长度相等    
					num[v] = num[v] + num[u];   //到达该城市方法就加上到达前一座城市的方法
					if (w[u] + weight[v] > w[v]) w[v] = w[u] + weight[v]; //比较两种方法的最大人数
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

---

可能待续 。。。   。。。
