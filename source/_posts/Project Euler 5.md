title: Project Euler 5 Smallest multiple（gcd）
date: 2014-10-27 11:30
tags: 
categories: [ACM, Project Euler]
toc: true
---
# 题目	
源地址：https://projecteuler.net/problem=5

# 题意
求一个最小的整数，使得该整数能够整除[1,20]的所有数

# 思路
lcm(a, b) = a * b / gcd(a, b)
要注意答案会超int
<!--more-->

# 代码
```
#include<iostream>
#include<cstdio>
#include<cstring>
#include<cmath>
#include<algorithm>
using namespace std;
#define ll long long
int main () {
    ll ans = 1;
    for (ll i = 1; i <= 20; i++) {
        ans = ans * i / __gcd(ans, i);
    }
    cout<<ans<<endl;
    return 0;
}
```

# 更新日志
- Completed on Mon, 27 Oct 2014, 06:20
