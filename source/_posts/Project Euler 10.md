title: Project Euler 10 Summation of primes（线性筛素数）
date: 2014-10-28 00:36
tags: [线性筛素数, 数论]
categories: [ACM, Project Euler]
toc: true
---
# 题目	
源地址：https://projecteuler.net/problem=10

# 题意
求所有小于2000000的素数和

# 思路
线性筛素数
<!--more-->

# 代码
```
#include<iostream>
#include<cmath>
#include<cstdio>
#include<algorithm>
using namespace std;
#define maxn 2000010
#define ll long long
int ans[maxn], valid[maxn], tot = 0;
ll sum = 0;
void getPrime(int n, int &tot, int ans[]) {
    for (int i = 2; i <= n; i++) {
        if (!valid[i]) ans[tot++] = i, sum += i;
        for (int j = 0; j < tot && i * ans[j] <= n; j++) {
            valid[i * ans[j]] = 1;
            if (i % ans[j] == 0) break;
        }
    }
}
int main () {
    sum = 0;
    getPrime(2000000, tot, ans);
    cout<<sum<<endl;
    return 0;
}
```

# 更新日志
- Completed on Mon, 27 Oct 2014, 12:03.18
