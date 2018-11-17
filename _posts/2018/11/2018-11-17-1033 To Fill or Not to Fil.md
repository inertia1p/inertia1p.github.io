---
layout:     post
title:      1033 To Fill or Not to Fill（25 分）贪心算法 ★
subtitle:   PAT重要题整理
date:       2018-11-17
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

1033 To Fill or Not to Fill （25 分）
With highways available, driving a car from Hangzhou to any other city is easy. But since the tank capacity of a car is limited, we have to find gas stations on the way from time to time. Different gas station may give different price. You are asked to carefully design the cheapest route to go.

Input Specification:
Each input file contains one test case. For each case, the first line contains 4 positive numbers: C
​max
​​  (≤ 100), the maximum capacity of the tank; D (≤30000), the distance between Hangzhou and the destination city; D
​avg
​​  (≤20), the average distance per unit gas that the car can run; and N (≤ 500), the total number of gas stations. Then N lines follow, each contains a pair of non-negative numbers: P
​i
​​ , the unit gas price, and D
​i
​​  (≤D), the distance between this station and Hangzhou, for i=1,⋯,N. All the numbers in a line are separated by a space.

Output Specification:
For each test case, print the cheapest price in a line, accurate up to 2 decimal places. It is assumed that the tank is empty at the beginning. If it is impossible to reach the destination, print The maximum travel distance = X where X is the maximum possible distance the car can run, accurate up to 2 decimal places.

Sample Input 1:
```
50 1300 12 8
6.00 1250
7.00 600
7.00 150
7.10 0
7.20 200
7.50 400
7.30 1000
6.85 300
```
Sample Output 1:
```
749.17
```
Sample Input 2:
```
50 1300 12 2
7.10 0
7.00 600
```
Sample Output 2:
```
The maximum travel distance = 1200.00
```
**难点：贪心算法**

## 思路

1. 在目的距离也设一个加油站，油价为0，这样就把问题同质化为找可达最大距离中找最便宜油价问题。
2. 因为初始油量为0，如果没有距离为0的加油站直接返回。
3. 主体判断过程： **在可达距离内找比当前节点price低的加油站**，如果有就加正好到达的油量，去下一个节点。如果没有就在当前节点加满油，去可达距离内price最低的节点。直到到达目的地
4. 到达距离后返回。

## 解题代码

```C++
#include<bits/stdc++.h>
using namespace std;
struct p {
	double dis,price;
};
bool cmp(p a, p b) {return a.dis < b.dis;}
int main() {
	double Cap, Dis, Davg, n, nowdis = 0, nowcap = 0, sum = 0,dis=0;
	cin >> Cap >> Dis >> Davg >> n;
	vector<p> station;
	station.push_back(p{ Dis,0 });
	for (int i = 0; i < n; i++) {
		double pr, d;
		cin >> pr >> d;
		station.push_back(p{ d,pr });
	}
	sort(station.begin(), station.end(), cmp);
	if (station[0].dis != 0) {
		cout << "The maximum travel distance = 0.00";
		return 0;
	}
	double range = Cap * Davg;
	int nowindex = 0;
	while (nowdis != Dis) {
		nowcap = nowcap - dis / Davg;
		double minprice = 9999999;
		int nextindex = nowindex;
		for (int i = nowindex + 1; i < station.size()&&station[i].dis<=station[nowindex].dis+range; i++) {
			if (station[i].price < station[nowindex].price) {
				nextindex = i;
				break;
			}
			if (minprice > station[i].price) {
				minprice = station[i].price;
				nextindex = i;
			}
		}
		if (nextindex == nowindex) {
			printf("The maximum travel distance = %.2f", station[nowindex].dis+range);
			return 0;
		}
		if (station[nextindex].price>station[nowindex].price) {
			sum += (Cap - nowcap) * station[nowindex].price;
			nowcap = Cap;
		}
		else {
			double need = (station[nextindex].dis - station[nowindex].dis)/Davg - nowcap;
			if (need > 0) sum += need * station[nowindex].price;
			nowcap += need;
		}
		dis = station[nextindex].dis - station[nowindex].dis;
		nowindex = nextindex;
		nowdis = station[nowindex].dis;
	}
	printf("%.2f", sum);
	return 0;
}
```
