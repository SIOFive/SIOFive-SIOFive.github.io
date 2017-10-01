title: SPOJ DQUERY - D-query（查询区间内有几个数 离线树状数组OR在线主席树）
date: 2015-10-22 13:40
tags: [数据结构, 树状数组, 主席树]
categories: [ACM]
toc: true
---
# 题目	
源地址：http://www.spoj.com/problems/DQUERY/en/

# 题意
查询区间内有几个不同的数

# 思路
http://blog.csdn.net/acm_cxlove/article/details/8562634

1. 离线树状数组
将查询区间按左端点排序
对于相同的数，先更新最左边的位置
然后根据查询区间，不断更新next，保证查询区间内只记录一个位置
维护前缀和用树状数组，时空效率都高

2. 在线主席树
http://www.cnblogs.com/Empress/p/4675386.html
将重复的元素建树。在query的时候把区间内重复的数加起来，用区间长度(r-l+1)去减就是答案
(query的是[l, r]之间重复元素的个数)
举个例子：1 1 2 1 3 2 3
我们从左到右枚举，如果没有重复的，那么树的形态和之前一样，出现重复的则在重复的位置加1，建一颗新树
这样我们需要建7颗数
    1 2 3 4 5 6 7
T0:
T1:
T2: 1
T3: 1
T4: 1 1
T5: 1 1
T6: 1 1 1
T7: 1 1 1  1
例如查询区间[2，5] 就需要用T5-T1，但是不能直接减，直接减重复的数为2个1。需要统计的是区间[2,5]内重复的个数，正好为1个1.

<!--more-->

# 代码
离线树状数组
```
#include<iostream>
#include<cmath>
#include<algorithm>
#include<cstring>
#include<string>
#include<cstdio>
#include<vector>
#include<map>
#define pb push_back
#define INF 1 << 30
#define fi first
#define se second
#define debug puts("=====================");
using namespace std;
typedef long long ll;
const int N = 30000 + 100;
const int M = 200000 + 100;
int a[N], s[N], n, nxt[N];
struct node {
    int l, r, id;
    bool operator < (const node & T) const {
        return l < T.l;
    }
} Q[M];
int ans[M];
int lowbit(int x) {
    return x & -x;
}
void add(int p, int v) {
    while(p <= n) {
        s[p] += v;
        p += lowbit(p);
    }
}
int sum(int p) {
    int res = 0;
    while(p > 0) {
        res += s[p];
        p -= lowbit(p);
    }
    return res;
}
map<int, int> mp;
int main () {
    while(~scanf("%d", &n)) {
        mp.clear();
        for (int i = 1; i <= n; i++) {
            scanf("%d", a + i);
            if (!mp.count(a[i])) {
                mp[a[i]] = i;
                add(i, 1);
            }
        }
        mp.clear();
        for (int i = n; i >= 1; i--) {
            if (!mp.count(a[i])) nxt[i] = n + 1;
            else nxt[i] = mp[a[i]];
            mp[a[i]] = i;
        }
        int q;
        scanf("%d", &q);
        for (int i = 0; i < q; i++) {
            scanf("%d%d", &Q[i].l, &Q[i].r);
            Q[i].id = i;
        }
        sort(Q, Q + q);
        int t = 1;
        for (int i = 0; i < q; i++) {
            while(t <= n && t < Q[i].l) {
                add(nxt[t++], 1);
            }
            ans[Q[i].id] = sum(Q[i].r) - sum(Q[i].l - 1);
        }
        for (int i = 0; i < q; i++) printf("%d\n", ans[i]);
    }
    return 0;
}
```

在线主席树
```
#include<iostream>
#include<cmath>
#include<algorithm>
#include<cstring>
#include<string>
#include<cstdio>
#include<vector>
#include<map>
#define pb push_back
#define INF 1 << 30
#define fi first
#define se second
#define debug puts("=====================");
using namespace std;
typedef long long ll;
typedef long long LL;
#define lson l, m
#define rson m + 1, r
const int N = 30000 + 10;
int ln[N << 5], rn[N << 5], sum[N << 5];
int tot, root[N], a[N];
map<int, int> mp;
int build(int l, int r) {
    int rt = ++tot;
    sum[rt] = 0;
    if (l < r) {
        int m = l + r >> 1;
        ln[rt] = build(lson);
        rn[rt] = build(rson);
    }
    return rt;
}
int update(int pre, int l, int r, int x) {
    int rt = ++tot;
    ln[rt] = ln[pre], rn[rt] = rn[pre], sum[rt] = sum[pre] + 1;
    if (l < r) {
        int m = l + r >> 1;
        if (x <= m) ln[rt] = update(ln[pre], lson, x);
        else rn[rt] = update(rn[pre], rson, x);
    }
    return rt;
}
int query(int u, int v, int l, int r, int k) {
    if (l >= k) return sum[v] - sum[u];
    int ans = 0;
    int m = l + r >> 1;
    if (k <= m) ans += query(ln[u], ln[v], lson, k);
    ans += query(rn[u], rn[v], rson, k);
    return ans;
}
int main () {
    int n, m;
    tot = 0;
    scanf("%d", &n);
    for (int i = 1; i <= n; i++) scanf("%d", a + i);
    root[0] = build(1, n);
    for (int i = 1; i <= n; i++) {
        if (mp.find(a[i]) != mp.end()) root[i] = update(root[i - 1], 1, n, mp[a[i]]);
        else root[i] = root[i - 1];
        mp[a[i]] = i;
    }
    scanf("%d", &m);
    int l, r;
    while(m--) {
        scanf("%d%d", &l, &r);
        printf("%d\n", r - l + 1 - query(root[l - 1], root[r], 1, n, l));
    }
    return 0;
}

```