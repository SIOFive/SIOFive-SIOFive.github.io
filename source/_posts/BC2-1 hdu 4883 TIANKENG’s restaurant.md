title: HDU 4883 TIANKENG's restaurant
date: 2015-7-19 20:05
tags: [想法]
categories: [ACM]
toc: true
---
# 题目	
源地址：http://acm.hdu.edu.cn/showproblem.php?pid=4883

# 题意
有一家餐厅，有n组人，每组包括Xi个人，在某一个时间段来到餐厅。问餐厅最少需要多少张凳子可以让每一个时刻所有在餐厅的人都有位子。

# 思路
把到达的和离开的时间按照从小到大排序，遇到到达时加上来的人数，遇到离开时减去离开的人数，遍历求出最大值即可。

<!--more-->

# 代码
```
#include<iostream>
#include<cstring>
#include<cstdio>
#include<cmath>
#include<algorithm>
using namespace std;
const int N = 10000 + 10;
int n, t;
pair<int, int> a[N + N];
int main () {
    scanf("%d", &t);
    int c, x, xx, y, yy, cnt;
    while(t--) {
        scanf("%d", &n);
        cnt = 0;
        for (int i = 0; i < n; i++) {
            scanf("%d%d:%d%d:%d", &c, &x, &xx, &y, &yy);
            a[cnt++] = make_pair(x * 60 + xx, c);
            a[cnt++] = make_pair(y * 60 + yy, -c);
        }
        sort(a, a + cnt);
        int ans = 0, sum = 0;
        for (int i = 0; i < cnt; i++) {
            sum += a[i].second;
            ans = max(ans, sum);
        }
        printf("%d\n", ans);
    }
    return 0;
}
```

# 更新日志
- 14040955  2015-07-17 16:01:31 Accepted    4883    577MS   1748K   643 B   G++ SIO__Five
