title: HDU 4886 TIANKENG’s restaurant(Ⅱ)
date: 2015-7-19 20:50
tags: [枚举, 哈希]
categories: [ACM]
toc: true
---
# 题目	
源地址：http://acm.hdu.edu.cn/showproblem.php?pid=4886

# 题意
有一个字符串S。只有ABCDEFGH这八个字符组成。现在需要找一个最小的由这八个字符组成的子串T，使其不是S的子串。
数据范围：1 ≤ n ≤ 1000000 其中n为S的长度

# 思路
假设结果长度为L，那么当8^L >= Length(S)时必有结果，所以可以确定L的最大值。暴力枚举长度为1，长度为2...长度为L。在枚举过程中，先对S中所有枚举长度的子串求hash值，然后再搜索对应长度的所有子串，O(1)判断即可。

<!--more-->

# 代码
```
#include<iostream>
#include<cstring>
#include<cstdio>
#include<cmath>
#include<algorithm>
using namespace std;
int T, n, len, a[100];
const int N = 1000000 + 100;
char s[N];
bool ha[N];
bool dfs(int k, int x) {
    //cout<<k<<" "<<x<<endl;
    if (k == len) {
        if (!ha[x]) return 1;
        return 0;
    }
    for (int i = 0; i < 8; i++) {
        a[k] = i;
        if (dfs(k + 1, x * 8 + i)) return 1;
    }
    return 0;
}
int main () {
    scanf("%d", &T);
    while(T--) {
        scanf("%s", s);
        n = strlen(s);
        for (len = 1; ; len++) {
            memset(ha, 0, sizeof(ha));
            for (int i = 0; i + len - 1 < n; i++) {
                int x = 0;
                for (int j = 0; j < len; j++)
                    x *= 8, x += s[i + j] - 'A';
                ha[x] = 1;
                //cout<<x<<endl;
            }
            if (!dfs(0, 0)) continue;
            for (int i = 0; i < len; i++)
                printf("%c", 'A' + a[i]);
            puts("");
            break;
        }
    }
    return 0;
}
```

# 更新日志
- 14049001  2015-07-18 12:23:03 Accepted    4886    1466MS  3536K   1092 B  G++ SIO__Five
