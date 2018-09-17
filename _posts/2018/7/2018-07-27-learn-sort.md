---
layout:     post
title:      算法基础————排序
subtitle:   PAT记录之路
date:       2018-07-27
author:     inertia1p
header-img: img/post-bg-sort.jpg
keywords_post:  "排序，算法"
catalog: true
tags:
    - 排序
    - 算法
---

>N为序列长度

>文章内gif均出自Jack Cui_blog

## 初识排序

#### 最熟悉的排序（暂）————冒泡

![bubble_sort](https://inertia1p.github.io/img_post/bubble_sort.gif)

冒泡排序是我接触的第一种排序。也是我现在唯一会使用的排序。他的写法为

``` c++
for(int i=0;i<N-1;i++){
  for(int j=0;j<N-j-1;j++){             /*这里已经做了一部分优化*/
    if(a[j]>a[j+1]){
      temp=a[j];
      a[j]=a[j+1];
      a[j+1]=temp;
    }
  }
}
```

~~os:简单好写。但是每次使用的时候总会感觉它会吃掉大量的时间。~~<br>

冒泡排序的平均时间复杂度为O（N^2）,是一种稳定的排序算法，序列最好情况是**正序**时间复杂度为O（N），最坏情况是**反序**时间复杂度为O（N^2）。  
其实冒泡排序还有优化算法。普通版在排序过程中，可能在某一趟排序完成后就已经是正序了，但是它还是会继续比较下去（只是没有了交换）。所以可以在算法中加入标志变量来标识某一趟排序中是否有数据交换。**标志变量改变说明排序可以立即结束**

```c++
for(int i=0,flag=0,temp=0;i<N-1;i++,flag=0){
  for(int j=0;j<N-1;j++){               /*使用标识时判断语句有所变化*/
    if(a[j]>a[j+1]){
      temp=a[j];
      a[j]=a[j+1];
      a[j+1]=temp;
      flag=1;
    }
  }
  if(flag==0) break;
}
/*这个写法比上面的写法循环次数要少。*/
```

#### PAT乙级题1035 插入和归并

这篇blog的引子就是来自PAT乙级题的1035。这部分就把插入和归并两种排序算法整理学习一下。

###### 插入排序

![insert_sort](https://inertia1p.github.io/img_post/insert_sort.gif)

插入排序的平均时间复杂度是O(n^2),是稳定的排序算法，序列最好情况是**正序**时间复杂度为O（N），最坏情况是**反序**时间复杂度为O（N^2）。插入排序从第二个元素开始，每次排序把前面的元素当做一个序列。但是因为每一次排序完成后，前面的序列都是正序的。所以，每一次排序都会从前一个元素开始与当前元素比较，如果满足序列要求，则把当前比较元素后移一位。因为后移会覆盖当前元素，所以需要把当前元素用temp记录。不满足条件时，这一次排序结束。总共有N-1次这样的排序。<br>
代码样例如下：

```c++
for (int i = 1; i < N; i++) {
  int temp=a[i];
  int j = i - 1;
  for (; j >=0&&a[j]>temp; j--) {
    a[j + 1] = a[j];
  }
/*内循环就是元素后移过程*/
  a[j + 1] = temp;
}
```

同样这个代码也可以优化，因为每次内循环中，前面的序列总是有序的，我们可以用二分查找代替遍历，加快找到正确位置的速度。

```c++
int binarysearch(int j,int a[],int i);

for (int i = 1; i < N; i++) {
  int b = binarysearch(a[i],a,i);
  if (b != -1){                       /*在前面所有元素后面，不需要移动*/
    int temp = a[i];
    int j = i - 1;
    while (j >= b){
      a[j + 1] = a[j];
      j--;
    }
    a[j + 1] = temp;
  }
}
```

~~os：但是实际上根本不会写这个这么麻烦的东西的吧！ZZ~~ <br>

###### 归并排序

![Merge_sort](https://inertia1p.github.io/img_post/Merge_sort.gif)

归并排序（Merge sort）采用分治（divide-and-conquer）策略。它的平均时间复杂度为O（nlog2n）。特别的是它的最坏情况和最好情况的时间复杂度是相同的。它的空间复杂度为O（n）。它是稳定的排序算法

```c++
#include<vector>
#include <iostream>
using namespace std;
void merge(vector<int> &a, int l, int m, int r, vector<int> temp);
void mergesort(vector<int> &a);
void Mergesort(vector<int> &a, vector<int> temp, int l, int r);

void merge(vector<int> &a,int l,int m,int r,vector<int> temp) {
	int L = l;
	int M = m + 1; int R = r;
	int k = 0;
	while (L <= m && M <= R) {
		if (a[L] <= a[M]) {
			temp[k++] = a[L++];
		}
		else {
			temp[k++] = a[M++];
		}
	}
	while (L <= m) {
		temp[k++] = a[L++];
	}
	while (M <= r) {
		temp[k++] = a[M++];
	}
	k = 0;
	while (l <= r) {
		a[l++] = temp[k++];
	}

}

void mergesort(vector<int> &a) {
	vector<int> temp(a.size());
	Mergesort(a, temp, 0, a.size() - 1);
}

void Mergesort(vector<int> &a, vector<int> temp, int l, int r) {
	if (l < r) {
		int m = (r + l)/2;
		Mergesort(a, temp, l, m);
		Mergesort(a, temp, m+1, r);
		merge(a, l, m, r, temp);
	}
}

int main()
{
	int N=9,c=0;
	vector<int> a;
	for (int i = 0; i < 9; i++) {
		cin >> c;
		a.push_back(c);
	}
	mergesort(a);
	for (auto val : a) {
		cout << val << ' ';
	}
	system("pause");
	return 0;
}
```

归并运算的写法更为复杂，上述代码中主要包括一个类似二叉树的递归分解，把主序列分为一个个小序列，然后通过比较排序（merge（））导入临时存储的序列结构。然后再会推入原序列存储。因为归并排序的形式就是一棵二叉树，它需要遍历的次数就是二叉树的深度，而根据完全二叉树的可以得出它的时间复杂度是O(nlog2n)。  
归并排序是一种排序速度较快的排序算法，平均比前两种算法都要高效。这里我们不得不提到同样两种优秀的排序算法————快速排序（quick sort）和堆排序（heap sort）。  
* 若从空间复杂度来考虑：首选堆排序，其次是快速排序，最后是归并排序。  
* 若从稳定性来考虑，应选取归并排序，因为堆排序和快速排序都是不稳定的。  
* 若从平均情况下的排序速度考虑，应该选择快速排序。   
接下来我准备来学习总结这两种排序算法。  

#### 高效但不稳定的排序————堆排序、快速排序和希尔排序

###### 堆排序

---

又经历了不少pat题目，大部分排序题是自己总结一个新的排序算法。这个排序总结暂时放置一下。
