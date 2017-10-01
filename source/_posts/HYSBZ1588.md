title: HYSBZ 1588 营业额统计（Splay树）
date: 2015-8-27 6:03
tags: [Splay]
categories: [ACM]
toc: true
---
# 题目	
源地址：http://www.lydsy.com/JudgeOnline/problem.php?id=1588

# 题意
在n天里，每天读入一个数，规定这个数的贡献值为与之前所有数字之差的绝对值的最小值，第一个数的贡献值为它本身。求总的值

# 思路
这道题算是伸展树的入门题。题目数据有个bug（即可能数字个数小于n，如果小于n就把少的数字全部当做0）

算法的参考资料：
http://blog.csdn.net/acm_cxlove/article/details/7815019
http://wenku.baidu.com/view/7f0ff024ccbff121dd3683ac.html
http://www.cnblogs.com/kuangbin/archive/2012/10/07/2714068.html
http://wenku.baidu.com/link?url=amATxXzsEjjYrr4xw7OnIht8Iv8J7tx-brB6cwpVKfvujPO4p0iOMI46TECbQ4catynY5nOrlRkf2ZGRroXfU2wpSHCnehQCBDq0TYp_ddy&qq-pf-to=pcqq.c2c

splay树的关键在于splay操作，每次插入一个节点后，都将该节点旋到根节点。均摊复杂度为O(logn)，可以避免普通二叉查找树在某些情况下退化成一条链
这道题相当于每次插入一个数字后，找到其前驱和后继，比较两者与其的差即可。

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
#define pb push_back
#define debug puts("=====================");
using namespace std;
const int N = 100005;
const int inf = 1e9;
struct SplayTree {
    int pre[N], key[N], ch[N][2], root, cnt;  ///分别表示父结点，键值，左右孩子(0为左孩子，1为右孩子),根结点，结点数量
	void init() {
		root = cnt = 0;
	}
	///新建一个结点
    void NewNode(int &r, int father, int k) {
        r = ++cnt;
        pre[r] = father;
        key[r] = k;
        ch[r][0] = ch[r][1] = 0;  ///左右孩子为空
    }
    ///旋转，kind为1为右旋，kind为0为左旋
    void Rotate(int x, int kind) {
        int y = pre[x];
        ///类似SBT，要把其中一个分支先给父节点
        ch[y][!kind] = ch[x][kind];
        pre[ch[x][kind]] = y;
        ///如果父节点不是根结点，则要和父节点的父节点连接起来
        if(pre[y])
            ch[pre[y]][ch[pre[y]][1] == y] = x;
        pre[x] = pre[y];
        ch[x][kind] = y;
        pre[y] = x;
    }
	///Splay调整，将根为r的子树调整为goal
    void Splay(int r, int goal) {
        while(pre[r]!=goal) {
            ///父节点即是目标位置，goal为0表示，父节点就是根结点
            if(pre[pre[r]] == goal)
                Rotate(r, ch[pre[r]][0] == r);
            else {
                int y = pre[r];
                int kind = ch[pre[y]][0] == y;
                ///两个方向不同，则先左旋再右旋
                if(ch[y][kind] == r) {
                    Rotate(r, !kind);
                    Rotate(r, kind);
                }
                ///两个方向相同，相同方向连续两次
                else {
                    Rotate(y, kind);
                    Rotate(r, kind);
                }
            }
        }
        ///更新根结点
        if(goal == 0) root = r;
    }
    int Insert(int k) {
        int r = root;
        while(ch[r][key[r] < k]) {
            ///不重复插入
            if(key[r] == k) {
                Splay(r,0);
                return 0;
            }
            r = ch[r][key[r] < k];
        }
        NewNode(ch[r][k > key[r]], r, k);
        ///将新插入的结点更新至根结点
        Splay(ch[r][k > key[r]], 0);
        return 1;
    }
	///找前驱，即左子树的最右结点
    int get_pre(int x) {
        int tmp = ch[x][0];
        if(tmp == 0)  return inf;
        while(ch[tmp][1])
            tmp = ch[tmp][1];
        return key[x] - key[tmp];
    }
    ///找后继，即右子树的最左结点
    int get_next(int x) {
        int tmp = ch[x][1];
        if(tmp == 0)  return inf;
        while(ch[tmp][0])
            tmp = ch[tmp][0];
        return key[tmp] - key[x];
    }
}sp;
int n;
int main() {
    while(~scanf("%d",&n)) {
        sp.init();
        int ans = 0;
        for(int i = 0; i < n; i++) {
            int num;
            if(scanf("%d",&num) == EOF) num = 0;
            if(!i) {
                ans += num;
                sp.NewNode(sp.root, 0, num);
                continue;
            }
            if (sp.Insert(num) == 0) continue;
            int a = sp.get_next(sp.root);
            int b = sp.get_pre(sp.root);
            ans += min(a, b);
        }
        printf("%d\n",ans);
    }
    return 0;
}
```
