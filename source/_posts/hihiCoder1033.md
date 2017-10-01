title: hihiCoder 1033 交错和（数位dp）
date: 2015-9-3 17:38
tags: [数位dp]
categories: [ACM]
toc: true
---
# 题目	
源地址：http://hihocoder.com/problemset/problem/1033

# 题意
规定f(x)为x的各位交错和，现在给定一个区间[l,r]，求区间内满足f(x)=k的所有x的和

# 思路
很明显是数位dp的思想，但是状态比较多。
dp[fh][one][pos][psum]来表示符号为fh，前缀零的状态one，当前位pos，以及该位以后的交错和为psum的个数
sum[fh][one][pos][psum]则表示所有这些数的和
之后的转移dp比较容易，个数累加即可，sum则相对复杂，详见代码
这里one这个状态很关键，因为当前面全为前缀零时，fh其实是没有意义的，所以如果不单独开一维维护one，会导致结果错误

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
#define pll pair<ll, ll>
ll l, r, k;
ll dp[2][2][20][400];
ll sum[2][2][20][400];
ll base[20];
int bit[20];
const int mod = 1e9 + 7;
pll dfs(int pos, int psum, bool fh, bool one, bool flag) {
    if (pos == -1) {
        if (psum == 0) return {1, 0};
        else return {0, 0};
    }
    int ss;
    if (!flag) {
        ss = psum + 200;
        if (dp[fh][one][pos][ss] != -1) return {dp[fh][one][pos][ss], sum[fh][one][pos][ss]};
    }
    int u = flag ? bit[pos] : 9;
    ll ans = 0, res = 0;
    pll tmp;
    for (int i = 0; i <= u; i++) {
        int nxt = flag && i == u;
        if (!i && one) {
            tmp = dfs(pos - 1, psum, 1, 1, nxt);
        } else {
            if (fh) tmp = dfs(pos - 1, psum + i, 0, 0, nxt);
            else tmp = dfs(pos - 1, psum - i, 1, 0, nxt);
        }
        ans = (ans + tmp.fi) % mod;
        res = (res + (i * base[pos] % mod * tmp.fi % mod) + tmp.se) % mod;
    }
    if (ss < 0) debug;
    if (!flag) dp[fh][one][pos][ss] = ans, sum[fh][one][pos][ss] = res;
    return {ans, res};
}
ll calc(ll x) {
    if (x < 0) return 0;
    int pos = 0;
    while(x) {
        bit[pos++] = x % 10;
        x /= 10;
    }
    pll tmp = dfs(pos - 1, -k, 1, 1, 1);
    return tmp.se;
}
int main () {
    base[0] = 1;
    for (int i = 1; i <= 19; i++) base[i] = base[i - 1] * 10 % mod;
    memset(dp, -1, sizeof(dp));
    while(~scanf("%lld%lld%lld", &l, &r, &k)) {
        printf("%lld\n", (calc(r) - calc(l - 1) + mod) % mod);
    }
    return 0;
}
```
