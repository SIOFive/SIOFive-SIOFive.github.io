title: Project Euler 6 Sum square difference（公式）
date: 2014-10-27 14:48
tags: [公式]
categories: [ACM, Project Euler]
toc: true
---
# 题目	
源地址：https://projecteuler.net/problem=6

# 题意
$f(x) = 1^2 + 2^2 + \cdots + n^2$
$g(x) = (1 + 2 + \cdots + n)^2$
求$g(x)-f(x)$

# 思路
$f(x) = \frac{n(n+1)(2n+1)}{6}$
<!--more-->

# 代码
```
#include<iostream>
#include<cmath>
#include<cstdio>
using namespace std;
#define ll long long
int main () {
    int n = 100;
    ll sum = (2 * n + 1) * n * (n + 1) / 6;
    ll sq = (1 + n) * n / 2;
    sq *= sq;
    cout<<sq - sum<<endl;
    return 0;
}
```

# 更新日志
- Completed on Mon, 27 Oct 2014, 06:45
