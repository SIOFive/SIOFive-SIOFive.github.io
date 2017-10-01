title: Acdream 1075 神奇的%系列三（广义Fibonacci数列找循环节）
date: 2015-9-21 13:40
tags: [广义Fibonacci数列找循环节, 数学]
categories: [ACM]
toc: true
---
# 题目	
源地址：http://acdream.info/problem?pid=1075

# 题意
我们定义一个f(n)函数，f(n) = a * f(n - 1) + b * f(n - 2), f(1) = c, f(2) = d.
问f(n)在模1000000007情况下的最小循环节。即求最小的m，使对任意的n有f(n) % 1000000007 = f(n + m) % 1000000007.

# 思路
http://blog.csdn.net/ACdreamers/article/details/25616461

c=a*a+4*b
c是模p的二次剩余时，枚举n=p-1的因子
c是模的二次非剩余时，枚举的n=(p+1)(p-1)因子

<!--more-->

# 代码
```
#include<iostream>
#include<cmath>
#include<algorithm>
#include<cstring>
#include<string>
#include<cstdio>
#include<vector>
#define pb push_back
#define INF 1 << 30
#define fi first
#define se second
#define debug puts("=====================");
using namespace std;
typedef long long ll;
const int maxn = 2;
const int maxm = 2;
const ll mod = 1000000007;
struct Matrix {
    int n, m;
    ll a[maxn][maxm];
    void clear() {
        n = m = 0;
        memset(a, 0, sizeof(a));
    }
    Matrix operator * (const Matrix &b) const { //实现矩阵乘法
        Matrix tmp;
        tmp.n = n;
        tmp.m = b.m;
        for (int i = 0; i < n; i++)
            for (int j = 0; j < b.m; j++) tmp.a[i][j] = 0;
        for (int i = 0; i < n; i++)
            for (int j = 0; j < m; j++) {
                if (!a[i][j]) continue;
                for (int k = 0; k < b.m; k++)
                    tmp.a[i][k] += a[i][j] * b.a[j][k], tmp.a[i][k] %= mod;
            }

        return tmp;
    }
    void Copy(const Matrix &b) {
        n = b.n, m = b.m;
        for (int i = 0; i < n; i++)
            for(int j = 0; j < m; j++) a[i][j] = b.a[i][j];
    }
    void unit(int sz) {
        n = m = sz;
        for (int i = 0; i < n; i++) {
            for (int j = 0; j < n; j++) a[i][j] = 0;
            a[i][i] = 1;
        }
    }
};
Matrix A, B;
Matrix Matrix_pow(Matrix A, ll k, ll mod) { //矩阵快速幂
    Matrix res;
    res.clear();
    res.n = res.m = 2;
    for (int i = 0; i < 2; i++) res.a[i][i] = 1;
    while(k) {
        if (k & 1) res.Copy(A * res);
        k >>= 1;
        A.Copy(A * A);
    }
    return res;
}

ll pow_mod(ll a, ll n, ll p) {
    ll res = 1;
    while (n) {
        if (n & 1) res = res * a % p;
        n >>= 1;
        a = a * a % p;
    }
    return res;
}
ll legendre(ll a, ll p) {
    return pow_mod(a, (p - 1) >> 1, p);
}

ll fac[2][500];
int cnt, ct;
ll pri[6] = {2, 3, 7, 109, 167, 500000003};
ll num[6] = {4, 2, 1, 2, 1, 1};
void dfs(int dep, ll pro) {
    if (dep == cnt) {
        fac[1][ct++] = pro;
        return ;
    }
    for (int i = 0; i <= num[dep]; i++) {
        dfs(dep + 1, pro);
        pro *= pri[dep];
    }
}
void init() {
    cnt = 6;
    ct = 0;
    dfs(0, 1);
    sort(fac[1], fac[1] + ct);
    fac[0][0] = 1;
    fac[0][1] = 2;
    fac[0][2] = 500000003;
    fac[0][3] = 1000000006;
}
bool check(Matrix A, ll n) {
    Matrix ans = Matrix_pow(A, n, mod);
    return (ans.a[0][0] == 1 && ans.a[0][1] == 0 && ans.a[1][0] == 0 && ans.a[1][1] == 1);
}
ll a, b, c, d;
int main () {
    init();
    while(~scanf("%lld%lld%lld%lld", &a, &b, &c, &d)) {
        ll t = a * a + 4 * b;
        A.n = A.m = 2;
        A.a[0][0] = a;
        A.a[0][1] = b;
        A.a[1][0] = 1;
        A.a[1][1] = 0;
        if (legendre(t, mod) == 1) {
            for (int i = 0; i < 4; i++) {
                if (check(A, fac[0][i])) {
                    printf("%lld\n", fac[0][i]);
                    break;
                }
            }
        } else {
            for (int i = 0; i < ct; i++) {
                if (check(A, fac[1][i])) {
                    printf("%lld\n", fac[1][i]);
                    break;
                }
            }
        }
    }
    return 0;
}
```
