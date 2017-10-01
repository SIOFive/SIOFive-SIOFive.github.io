title: Codeforces 55D Beautiful numbers（数位DP）
date: 2015-08-16 02:23
tags: [数位dp]
categories: [ACM]
toc: true
---
# 题目	
源地址：http://codeforces.com/problemset/problem/55/D

# 题意
给定一个数字n，如果n能被它数位上的每一个非零数字整除，这个数就被称为Beautiful number。为给定区间[l, r]内有多少个Beautiful numbers。其中1≤l≤r≤9*1e18

# 思路
一个数能被其所有数位上的非零数字整除，即被他们的最小公倍数整除，而1~9的lcm为2520。所以数位dp时只需要保存前面那些位的最小公倍数即可，到边界就把所有位的lcm求出了，为了判断这个数能不能被lcm整除，还需要保存这个数的值，显然记录是不行的。其实只要记录这个数模2520即可。这样可以设计如下dp：dfs(pos,mod,lcm,f)。pos为当前位，mod为模2520的值，lcm为最小公倍数，f表示前面是否达到上限。但其实1~9的所有最小公倍数只有48个。这样开dp[20][2520][48]的数组即可。

<!--more-->

# 代码
标称
```
#include<iostream>
#include<cmath>
#include<algorithm>
#include<cstring>
#include<string>
#include<cstdio>
#include<vector>
#include<queue>
#define pb push_back
#define debug puts("=====================");
using namespace std;
typedef long long ll;
const int mod = 2520;
ll dp[20][mod][48];
ll l, r;
int num[mod + 10], cnt, t, bit[20];
void init() {
	for (int i = 1; i <= mod; i++) if (mod % i == 0) {
		num[i] = cnt++;
	}
}
int gcd(int x, int y) {
	if (!y) return x;
	return gcd(y, x % y);
}
int lcm(int x, int y) {
	return x / gcd(x, y) * y;
}
ll dfs(int pos, int pre, int prelcm, bool flag) {
	if (pos == -1) return (pre % prelcm == 0);
	ll ret = dp[pos][pre][num[prelcm]];
	if (!flag && ret != -1) return ret;
	int u = flag ? bit[pos] : 9;
	ret = 0;
	for (int i = 0; i <= u; i++) {
		int now = (pre * 10 + i) % mod;
		int nowlcm = prelcm;
		if (i) nowlcm = lcm(prelcm, i);
		ret += dfs(pos - 1, now, nowlcm, (i == u ? 1 : 0) && flag);
	}
	if (!flag) dp[pos][pre][num[prelcm]] = ret;
	return ret;
}
ll calc(ll x) {
	int pos = 0;
	while(x) {
		bit[pos++] = x % 10;
		x /= 10;
	}
	return dfs(pos - 1, 0, 1, 1);
}
int main () {
	init();
	memset(dp, -1, sizeof(dp));
	scanf("%d", &t);
	while(t--) {
		scanf("%I64d%I64d", &l, &r);
		printf("%I64d\n", calc(r) - calc(l - 1));
	}
	return 0;
}
```

