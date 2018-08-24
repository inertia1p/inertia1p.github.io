---
layout:     post
title:      PAT中C++技巧总结
subtitle:   错误是攀登的阶梯
date:       2018-08-07
author:     inertia1p
header-img: img/post-bg-rwd.jpg
keywords_post:  "PAT，C++"
catalog: true
tags:
    - C++
    - PAT
---

## IO 流

1.getline（）它会读取在缓冲区的所有字符直到读取到'\n'。所以如果你上一行有进行输入，记得使用getchar()吸掉行尾的'\n'。  

2.sscanf  
例：取/和@之间字符  

```c++
const char* s = "iios/12DDWDFF@122";
　　char buf[20];
　　sscanf( s, "%*[^/]/%[^@]", buf );
　　printf( "%s/n", buf );
```

' * '是忽略符号,'[]'遵循贪婪匹配,例如[A-Z]表示匹配字符A-Z，这个正则方法在scanf中也适用。'^'是取补集的意思，如果取到^后的字符，sscanf就停止。sscanf的返回值为成功转换分配的字符数。  

3.sprintf  
不同于sscanf可以把字符串转换为其他类型，sprintf是把其他类型转换回字符串。  
`int sprintf(char *str, const char *format, ...) `  
例：

```C++
#include <stdio.h>
#include <math.h>

int main()
{
   char str[80];

   sprintf(str, "Pi 的值 = %f", M_PI);
   puts(str);

   return(0);
}
```
>sscanf和sprintf都是属于C的方法，所以不能使用C++的string类，需要的参数是char类型的指针。

## 字符串

1.find的使用  
.find(&string,起始下标（可不加）)，返回第一次找到的下标值。如果没找到，返回string：：npos，所以判断语句这样写`if(a.find('zhao')!=string::npos){}`这样是找到了字符串'zhao'。  

2.stoi，stol，stod，stoll..  
把string转化为int，long，double，long long。这些是它们各自的返回值，很简单的方法。

## 暂列单

 \*max_element(::iter,::iter);
