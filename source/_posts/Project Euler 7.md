title: Project Euler 7 10001st prime（筛素数）
date: 2014-10-27 17:45
tags: [数论, 线性筛素数]
categories: [ACM, Project Euler]
toc: true
---
# 题目	
源地址：https://projecteuler.net/problem=7

# 题意
求第10001个素数

# 思路
采用线性筛素数，复杂度为O(N)
<!--more-->

# 代码
```
#include<iostream>
#include<cstdio>
#include<cmath>
#include<algorithm>
using namespace std;
#define maxn 1000000
int ans[maxn], valid[maxn], tot = 0;
void getPrime(int n, int &tot, int ans[]) {
    for (int i = 2; i <= n; i++) {
        if (!valid[i]) ans[tot++] = i;
        for (int j = 0; j < tot && i * ans[j] <= n; j++) {
            valid[i * ans[j]] = 1;
            if (i % ans[j] == 0) break;
        }
    }
}
int main () {
    getPrime(999999, tot, ans);
    cout<<ans[10000]<<endl;
    return 0;
}
```

# 更新日志
- Completed on Mon, 27 Oct 2014, 09:36
