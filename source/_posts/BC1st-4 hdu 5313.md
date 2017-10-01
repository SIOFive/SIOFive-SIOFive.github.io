title: HDU 5313 Bipartite Graph（二分图染色, dp, bitset）
date: 2015-7-27 20:14
tags: [二分图染色, dp, bitset]
categories: [ACM]
toc: true
---
# 题目	
源地址：http://acm.hdu.edu.cn/showproblem.php?pid=5313

# 题意
给定一张图，有n个点，m条边，保证其连通的部分是二分图。问要使该图成为完全二分图，问最多能够加多少条边。
其中2≤n≤10000,0≤m≤100000

# 思路
首先，当点数固定时，两点之间越接近，完全二分图的边数越大。所以对每一个子图进行二分图染色，得到[ai, bi]的对。其中在X部有ai点，在Y部有bi点。把单一的点处理成[1, 0]。
用bitset<N>dp来表示状态，一开始dp[0] = 1，表示X部选了0个点是可行的。然后每次考虑pair对，转移方程为dp = (dp << ai) | (dp << aj)。即X部可以加上ai或者aj个点。最后选择差值最小的即可。

我比赛的时候用set来处理，结果在BC的评测平台上T了。赛后HDU上过了。。
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
#include<queue>
#include<set>
#include<bitset>
#define pb push_back
#define debug puts("=====================");
using namespace std;
const int maxn = 10000 + 10;
int n, m;
bool has[maxn];
int in[maxn];
int od, ev, cc;
vector<int> g[maxn];
pair<int, int> gg[maxn];
void dfs(int u) {
    for (int i = 0; i < g[u].size(); i++) {
        int v = g[u][i];
        if (!in[v]) {
            in[v] = in[u] + 1;
            if (in[v] == 3) in[v] = 1;
            if (in[v] == 2) ev++;
            else od++;
            dfs(v);
        }
    }
}
bitset<maxn> dp;
int main () {
    int t;
    scanf("%d", &t);
    while(t--) {
        scanf("%d%d", &n, &m);
        for (int i = 1; i <= n; i++) g[i].clear(), has[i] = false, in[i] = 0;
        int u, v ;
        for (int i = 0; i < m; i++) {
            scanf("%d%d", &u, &v);
            g[u].pb(v), g[v].pb(u);
            has[u] = has[v] = true;
        }
        int cc = 0;
        for (int u = 1; u <= n; u++) if (!in[u] && has[u]) {
                od = 1, ev = 0;
                in[u] = 1;
                dfs(u);
                gg[cc++] = {od, ev};
        }
        //bitset优化
        dp[0] = 1;
        for (int i = 1; i <= n; i++) if (!has[i]) gg[cc++] = {1, 0};
        for (int i = 0; i < cc; i++)
            dp = (dp << gg[i].first) | (dp << gg[i].second);
        int ans;
        for (int i = n/ 2; i >= 0; i--){
            if (dp[i]) {
                ans = i;
                break;
            }
        }
        printf("%d\n", ans * (n - ans) - m);
    }
    return 0;
}
```

# 更新日志
- 14206240  2015-07-27 20:14:49 Accepted    5313    546MS   3280K   1767 B  C++ SIO__Five
