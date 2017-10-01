title: Project Euler 2 Even Fibonacci numbers（递推）
date: 2014-10-27 10:00
tags: 
categories: [ACM, Project Euler]
toc: true
---
# 题目	
源地址：https://projecteuler.net/problem=2

# 题意
求不超过4000000的菲波那切偶数和

# 思路
递推

<!--more-->
# 代码
```
/*
    所有不超过4000000的菲波那切偶数和
*/
#include<iostream>
#include<cstdio>
#include<cmath>
using namespace std;
int f[40];
int main () {
    f[0] = 1, f[1] = 2;
    int sum = 2;
    for (int i = 2; i < 40; i++) {
        f[i] = f[i - 1] + f[i - 2];
        if (f[i] > 4000000) break;
        if (f[i] % 2 == 0) sum += f[i];
    }
    cout<<sum<<endl;
    return 0;
}
```

# 更新日志
- 2014-10-27 AC
- 开始做Project Euler
