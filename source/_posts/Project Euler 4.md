title: Project Euler 4 Largest palindrome product（枚举）
date: 2014-10-27 11:00
tags: 
categories: [ACM, Project Euler]
toc: true
---
# 题目	
源地址：https://projecteuler.net/problem=4

# 题意
求最大的回文数，该数字由两个三位数相乘得到。

# 思路
枚举两个三位数即可
<!--more-->

# 代码
```
#include<iostream>
#include<cstdio>
#include<cstring>
#include<cmath>
#include<algorithm>
using namespace std;
bool check(int x) {
    string s = "";
    while(x) {
        s += x % 10 + '0';
        x /= 10;
    }
    for (int i = 0, j = s.length() - 1; i < j; i++, j--) if (s[i] != s[j]) return false;
    return true;
}
int main () {
    int mx = 0;
    for (int i = 999; i >= 100; i--) {
        for (int j = i; j >= 100; j--) {
            int t = i * j;
            if (t <= mx) break;
            if (check(t)) mx = t;
        }
    }
    cout<<mx<<endl;
    return 0;
}
```

# 更新日志
- Completed on Mon, 27 Oct 2014, 03:41
