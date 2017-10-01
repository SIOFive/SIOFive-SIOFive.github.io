title: HDU 4884 TIANKENG's rice shop（模拟）
date: 2015-7-19 20:10
tags: [模拟]
categories: [ACM]
toc: true
---
# 题目	
源地址：http://acm.hdu.edu.cn/showproblem.php?pid=4884

# 题意
有一家餐厅，有n种炒饭，每次炒一次饭需要t时间，其中炒一次饭有k碗。有m个人来到在某一个时间来到餐厅，他点了num碗第id种的炒饭。问每一个客人最早离开餐厅的时间。
在炒饭过程中，保证先来先服务。但是每次炒会炒尽可能多的份数，不过不会有多余的。


# 思路
这道模拟题特别恶心。。
举个例子，比如每次可以炒5份，每次5分钟。
第一个顾客08:00进来，点了2份A，
第二个顾客08:04进来，点了3份A。
在08:00开始炒的话，由于这个时候第二个顾客还没进来，所以就只炒2份，第一个顾客在08:05离开，这时才炒第二个的3份，所以第二个离开时间是08:10。

同样是每次可以炒5份，每次5分钟。
第一个顾客08:00进来，点了6份A，
第二个顾客08:01进来，点了5份B，
第三个顾客08:02进来，点了4份A。
同样地，先炒5份给第一个，还差一份，这是已经是08:05了，第三个顾客也进来了，所以这时直接炒5份A（因为会尽可能多地炒），08：10第一个和第三个可以同时离开。接着才炒第二个的。

我的做法时，每次服务某个客人时，判断最多能够多炒几份饭，然后分给后来的客人。
这道题还有一个坑的地方是，客人最后离开的时间可能会超过24小时，需要取模。

<!--more-->

# 代码
```
#include<iostream>
#include<cstring>
#include<cstdio>
#include<cmath>
#include<algorithm>
using namespace std;
int T, n, t, k, m;
int a[1010], id[1010], num[1010], out[1010], kind[1010];
int main () {
    scanf("%d", &T);
    while(T--) {
        scanf("%d%d%d%d", &n ,&t, &k, &m);
        //memset(kind, 0, sizeof(kind));
        int x, y, now = 0;
        for (int i = 0; i < m; i++) {
            scanf("%d:%d%d%d", &x, &y, &id[i], &num[i]);
            a[i] = x * 60 + y;
        }
        for (int i = 0; i < m; i++) {
            if (!i) now = 0;
            else now = max(now, a[i - 1]);
            //cout<<num[i]<<endl;
            if (!num[i]) continue;
            now = out[i] = max(now, a[i]) + (num[i] + k - 1) / k * t;
            int tmp = k - num[i] % k;
            if (tmp == k) continue;
            for (int j = i + 1; j < m; j++) {
                if (a[j] > out[i] - t) break;
                if (id[j] == id[i]) {
                    if (num[j] <= tmp) {
                        tmp -= num[j];
                        num[j] = 0;
                        out[j] = max(out[i], a[j]);
                    } else {
                        num[j] -= tmp;
                        tmp = 0;
                    }
                    //cout<<j<<"-------------"<<tmp<<endl;
                    if (!tmp) break;
                }
            }
        }
        for (int i = 0; i < m; i++) printf("%02d:%02d\n", (out[i] % 1440) / 60, (out[i] % 1440) % 60);
        if (T) puts("");
    }
    return 0;
}
```

# 更新日志
- 14055192  2015-07-18 21:42:51 Accepted    4884    93MS    1608K   1565 B  G++ SIO__Five
