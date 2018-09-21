---
layout:     post
title:      1034 Head of a Gang 错误示范
subtitle:   找不到错误是真的很烦————鲁迅没说过
date:       2018-09-14
author:     inertia1p
header-img: img/post-bg-github-cup.jpg
keywords_post:  "C++，图，PAT"
catalog: true
tags:
    - C++
    - PAT
    - 图
---

## 题目大意

题目大意：给出1000条以内的通话记录A B和权值w，和阈值k，如果一个团伙人数超过2人并且通话总权值超过k，令团伙里面的自身权值的最大值为头目，输出所有满足条件的团伙的头目，和他们团伙里面的人数。

## 我的错误示范

```c++

// PATtest.cpp: 定义控制台应用程序的入口点。
//

#include "stdafx.h"
#include <iostream>
#include <algorithm>
#include <vector>
#include<string>
#include<map>
using namespace std;

int main()
{
	int N, K;
  //用二维vector记录所有小团体（可能不是gang）
	vector<vector<string>> gang(1);
  //用map记录每一个人的通话时间
	map<string,int> m;
  //用map记录输出老大名字和gang内成员
	map<string, int> out;
	cin >> N >> K;
	for (int i = 0; i < N; i++) {
		string a, b;
		int t,flag=0;
		cin >> a >> b>>t;
		m[a] += t; m[b] += t;
    //第一次输入必有一个小团体生成
		if (i == 0) {
			vector<string> v(2);
			v[0] = a; v[1] = b;
			gang[0]=v;
		}
		else {

      //通过遍历所有小团体中成员判断是否关联了已有团体
			for (int j = 0; j < gang.size(); j++) {
				if (find(gang[j].begin(), gang[j].end(), a) == gang[j].end()) {
					if (find(gang[j].begin(), gang[j].end(), b) != gang[j].end()) {
						gang[j].push_back(a); flag = 1; break;
					}
				}
				else {
					if (find(gang[j].begin(), gang[j].end(), b) == gang[j].end()) {
						gang[j].push_back(b); flag = 1; break;
					}
					else flag = 1;
				}
			}

      //如果没有，那么就新生成一个小团体（必是两人规模）加入
			if (flag == 0) {
				vector<string> v(2);
				v[0] = a; v[1] = b;
				gang.push_back(v);
			}
		}
	}
  //输入和分配结束

  //遍历所有小团体
	for (int i = 0; i < gang.size(); i++) {
    //首先团体人数需要大于2
		if (gang[i].size()>2) {
			int max = -1; int sum = 0;
			string o;
      //团体总通话时间等于团体内每个人通话时间和 除以2
			for (int j = 0; j < gang[i].size(); j++) {
				sum += m[gang[i][j]];
				if (m[gang[i][j]] > max) {
          //把团体内通话时间最长的人找出
					max = m[gang[i][j]];
					o = gang[i][j];
				}
			}
      //如果团体总通话时间大于阈值K，把老大记录到OUT
			if (sum/2 > K) {
				out[o] = gang[i].size();
			}
		}
	}

  //输出OUT
  cout << gang.size() << endl;
  map<string, int>::iterator i = out.begin();
  while (i != out.end()) {
    cout << i->first << ' ' << i->second << endl;
    i++;
  }
	system("pause");
	return 0;
}
```


这个是我最初的想法，最后有一个测试点过不去，我现在贼烦。看了下大神的操作，发现这一题其实是图的题目，我想想还真是很有道理。先把我这个不成熟的想法记录在这里，再重新用图的操作解一遍这一道题。

~~话说我真的觉得我这个想法也挺对的。。。。。~~  


## 重做代码

```c++
// PATtest.cpp: 定义控制台应用程序的入口点。
//

#include "stdafx.h"
#include <iostream>
#include <algorithm>
#include <vector>
#include<string>
#include<map>
using namespace std;
map<int, string> itos;
map<string,int> stoint;
map<int, int> member;
int N,id = 1,K,cnt=0,sum=0;

int exchange(string s) {
	if (stoint[s] == 0) {
		stoint[s] = id;
		itos[id] = s;
		return id++;
	}
	else {
		return stoint[s];
	}
}

int relation[2010][2010];
bool chek[2010];

void DFS(int t) {
	chek[t] = true;
	for (int i = 1; i <= N * 2; i++) {
		if (relation[t][i] != 0) {
			sum += relation[t][i];
			member[t] += relation[t][i];
			member[i] += relation[t][i];
			relation[t][i] = relation[i][t] = 0;
			DFS(i);
		}
	}
}

int main()
{
	map<string, int> out;
	cin >> N >> K;
	for (int i = 0; i < N; i++) {
		string a, b; int t;
		cin >> a >> b >> t;
		exchange(a);
		exchange(b);
		relation[stoint[a]][stoint[b]] += t;
		relation[stoint[b]][stoint[a]] += t;
	}
	for (int i = 1; i <= itos.size(); i++) {
		if (!chek[i]) {
			DFS(i);
			if (member.size() > 2&&sum>K) {
				int max=0;
				int head;
				for (map<int, int>::iterator it = member.begin(); it != member.end(); it++) {
					if (it->second > max) {
						head = it->first;
						max = it->second;
					}
				}
				out[itos[head]]=member.size();
			}member.clear(); cnt = 0; sum = 0;
		}
	}
	cout << out.size()<<endl;
	for (map<string, int>::iterator it = out.begin(); it != out.end(); it++) {
		cout << it->first << ' ' << it->second << endl;
	}
	system("pause");
	return 0;
}
```

总觉得每次在编写代码时会在是否要新开辟空间存储数据上有犹豫。我总是倾向于使用更小的空间。但是事实上有时就是需要这样一个空间存储类标记数据。这样的思维方式有时会限制自己尽快得到正确的解。记得读'小工到专家'的时候这样写到：编写程序的时候应该注重于低耦合，还要为后来的更新留下空间。也就是说，我是这样理解的每次写下的代码在写的时候就不要想着这一次的代码是完美的。只有尝试了，才能像铸造台阶一样一步步精炼自己写下的代码。写过的代码可以更加促进理解实际问题的关键节点和细节问题。


## 样例代码

> 引用自柳婼 の blog

> https://www.liuchuo.net/archives/2331

```c++
#include <iostream>
#include <map>
using namespace std;
map<string, int> stringToInt;
map<int, string> intToString;
map<string, int> ans;
int idNumber = 1, k;
int stoifunc(string s) {
    if(stringToInt[s] == 0) {
        stringToInt[s] = idNumber;
        intToString[idNumber] = s;
        return idNumber++;
    } else {
        return stringToInt[s];
    }
}
int G[2010][2010], weight[2010];
bool vis[2010];
void dfs(int u, int &head, int &numMember, int &totalweight) {
    vis[u] = true;
    numMember++;
    if(weight[u] > weight[head])
        head = u;
    for(int v = 1; v < idNumber; v++) {
        if(G[u][v] > 0) {
            totalweight += G[u][v];
            G[u][v] = G[v][u] = 0;
            if(vis[v] == false)
                dfs(v, head, numMember, totalweight);
        }
    }
}
void dfsTrave() {
    for(int i = 1; i < idNumber; i++) {
        if(vis[i] == false) {
            int head = i, numMember = 0, totalweight = 0;
            dfs(i, head, numMember, totalweight);
            if(numMember > 2 && totalweight > k)
                ans[intToString[head]] = numMember;
        }
    }
}
int main() {
    int n, w;
    cin >> n >> k;
    string s1, s2;
    for(int i = 0; i < n; i++) {
        cin >> s1 >> s2 >> w;
        int id1 = stoifunc(s1);
        int id2 = stoifunc(s2);
        weight[id1] += w;
        weight[id2] += w;
        G[id1][id2] += w;
        G[id2][id1] += w;
    }
    dfsTrave();
    cout << ans.size() << endl;
    for(auto it = ans.begin(); it != ans.end(); it++)
        cout << it->first << " " << it->second << endl;
    return 0;
}

```

她的代码在运行速度上比我的有一些小优势。
