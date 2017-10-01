title: HDU 5312 Sequence（想法, 三角形数）
date: 2015-7-27 21:45
tags: [想法, 三角形数]
categories: [ACM]
toc: true
---
# 题目	
源地址：http://acm.hdu.edu.cn/showproblem.php?pid=5312

# 题意
有一个数列，第n(n≥1)项是3n*(n-1)+1. 现在他想知道对于一个给定的整数m, 是否可以表示成若干项上述数列的和. 如果可以, 那么需要的最小项数是多少?
其中1≤m≤1e9

# 思路
![](http://siofive.qiniudn.com/hdu5312.png)
神奇：一个自然数最多只需要3个三角形数即可表示。
我一开始用mp处理，然后T了。。mp果然速度慢。
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
#include<map>
#define pb push_back
#define debug puts("=====================");
using namespace std;
const int N = 200000;
int t, m, cnt;
int a[N];
int main () {
    scanf("%d", &t);
    for (int i = 1; ; i++) {
        a[cnt++] = 3 * i * (i - 1) + 1;
        if (a[cnt - 1] > 1e9) break;
    }
    while(t--) {
        scanf("%d", &m);
        if (m % 6 == 0) {
            puts("6");
            continue;
        }
        if (m % 6 == 1) {
            int id = lower_bound(a, a + cnt, m) - a;
            if (a[id] == m) puts("1");
            else puts("7");
            continue;
        }
        bool flag = false;
        if (m % 6 == 2) {
            for (int i = 0; 2 * a[i] <= m; i++) {
                int id = lower_bound(a, a + cnt, m - a[i]) - a;
                if (a[id] == m - a[i]) {
                    puts("2");
                    flag = true;
                    break;
                }
            }
            if (!flag) puts("8");
            continue;
        }
        printf("%d\n", m % 6);
    }
    return 0;
}
```

# 更新日志
- 14208127  2015-07-27 21:45:34 Accepted    5312    1014MS  1656K   1075 B  G++ SIO__Five
