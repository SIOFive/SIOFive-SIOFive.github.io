title: HDU 1698 Just a Hook（线段树）
date: 2014-10-29 22:06
tags: [数据结构, 线段树]
categories: [ACM]
toc: true
---
# 题目	
源地址：http://acm.hdu.edu.cn/showproblem.php?pid=1698

# 题意
给出一个有$n$个元素的数组，有$q$次操作：
	set(L, R, v)把区间{L, R}的值全部修改为 $v$(1,2,3)。
一开始所有值都为1。求最后所有元素的和。

# 思路
线段树成段更新，要理解lazy的使用。update：成段替换
<!--more-->

# 代码
```
#include<iostream>
#include<cstdio>
#include<cstring>
#include<algorithm>
using namespace std;
#define lson l, m, rt << 1
#define rson m + 1, r, rt << 1 | 1
const int maxn = 111111;
int sum[maxn << 2], col[maxn << 2];
void pushup(int rt) {
    sum[rt] = sum[rt << 1] + sum[rt << 1 | 1];
}
void pushdown(int m, int rt) {
    if (col[rt]) {
        col[rt << 1] = col[rt << 1 | 1] = col[rt];
        sum[rt << 1] = (m - (m >> 1)) * col[rt];
        sum[rt << 1 | 1] = (m >> 1) * col[rt];
        col[rt] = 0;
    }
}
void build(int l, int r, int rt) {
    col[rt] = 0;
    if (l == r) {
        sum[rt] = 1;
        return ;
    }
    int m = (l + r) >> 1;
    build(lson);
    build(rson);
    pushup(rt);
}
void update(int L, int R, int c, int l, int r, int rt) {
    if (L <= l && r <= R) {
        col[rt] = c;
        sum[rt] = (r - l + 1) * c;
        return ;
    }
    pushdown(r - l + 1, rt);
    int m = (l + r) >> 1;
    if (L <= m) update(L, R, c, lson);
    if (R > m) update(L, R, c, rson);
    pushup(rt);
}
int main () {
    int T, n, q;
    scanf("%d", &T);
    for (int cas = 1; cas <= T; cas++) {
        scanf("%d%d", &n, &q);
        build(1, n, 1);
        int x, y, z;
        while(q--) {
            scanf("%d%d%d", &x, &y, &z);
            update(x, y, z, 1, n, 1);
        }
        printf("Case %d: The total value of the hook is %d.\n", cas, sum[1]);
    }
    return 0;
}
```

# 更新日志
- 12011666	2014-10-29 22:01:13	Accepted	1698	687MS	2288K	1458 B	C++	SIO__Five
