title: Project Euler 9 Special Pythagorean triplet（枚举）
date: 2014-10-27 19:51
tags: 
categories: [ACM, Project Euler]
toc: true
---
# 题目	
源地址：https://projecteuler.net/problem=9

# 题意
$a^2+b^2=c^2$
$a+b+c=1000$
且$a<b<c$
求$abc$

# 思路
直接枚举a,b,c即可
<!--more-->

# 代码
```
#include<iostream>
#include<cmath>
#include<cstdio>
#include<algorithm>
using namespace std;
void work() {
    for (int i = 1; i <= 1000 / 3; i++) {
        for (int j = i + 1; j <= (1000 - i) / 2; j++) {
            int k = 1000 - i - j;
            if (i * i + j * j == k * k) {
                cout<<i * j * k <<endl;
                return ;
            }
        }
    }
}
int main () {
    work();
    return 0;
}
```

# 更新日志
- Completed on Mon, 27 Oct 2014, 11:46
