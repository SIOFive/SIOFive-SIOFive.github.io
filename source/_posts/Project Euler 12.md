title: Project Euler 12 Highly divisible triangular number（质因数分解）
date: 2014-10-28 00:43
tags: [质因数分解, 数论]
categories: [ACM, Project Euler]
toc: true
---
# 题目	
源地址：https://projecteuler.net/problem=12

# 题意
规定$f(n)=1+2+\cdots+n$
求最小的$f(n)$使得其因子个数超过500

# 思路
$N=2^{a_1}3^{a_2}5^{a_3}\cdots$ 的因子个数为 $(a_1+1)(a_2+1)(a_3+1)\cdots$
而$f(n)=\frac{n(1+n)}{2}$ ，且相邻的两个数一定互质，所以$f(n)$的因子个数 = $\frac{n}{2}$的因子个数 $*(1+n)$的因子个数（n为偶数）

<!--more-->

# 代码
```
#include<iostream>
#include<cmath>
#include<cstdio>
#include<algorithm>
using namespace std;
#define maxn 1111111
int a[maxn], b[maxn], tot = 0;
void factor(int n, int a[], int b[], int &tot) {
    int tmp, now;
    tmp = (int)(sqrt(n) + 1);
    tot = 0;
    now = n;
    for (int i = 2; i <= tmp; i++) if (now % i == 0) {
        a[tot] = i, b[tot] = 0;
        while(now % i == 0) {
            b[tot]++;
            now /= i;
        }
        tot++;
    }
    if (now != 1) a[tot] = now, b[tot++] = 1;
}
int f[maxn] = {0};
void get(int x) {
    f[x] = 1;
    factor(x, a, b, tot);
    for (int i = 0; i < tot; i++) f[x] *= b[i] + 1;
}
int main () {
    int x, y;
    f[1] = 1, f[2] = 2;
    for (int i = 3; i; i++) {
        if (i % 2 == 0) x = i / 2, y = i + 1;
        else x = (i + 1) / 2, y = i;
        if (!f[x]) get(x);
        if (!f[y]) get(y);
        int ans = f[x] * f[y];
        if (ans > 500) {
            cout<<(x * y)<<endl;
            break;
        }
    }
    return 0;
}
```

# 更新日志
- Completed on Mon, 27 Oct 2014, 13:54.49
