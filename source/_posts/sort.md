title: 排序
author: Monody12
tags:
  - sort
categories:
  - algorithm
date: 2021-07-29 13:16:00
---

## AquaMoon and Strange Sort （排序 思维题）
[原题链接](https://codeforces.com/contest/1546/problem/C)

### 题意
给定一个序列，将这个序列按从小到大来排序。可以通过相邻元素直接调换位置来实现排序。如果每个元素调换位置的次数都是偶数，则输出YES否则输出NO。序列中的元素值可以是相同的。

### 思考
显而易见的规律：一个元素调换位置的次数取决于它的始末位置，如果它其实位置在奇数位，那么排序结束后它的位置也在奇数位，那么调换位置的次数就为偶数。

但是我们不能仅通过观察一个元素的始末位置之差为奇数就说这个序列不满足条件。因为序列中是可以存在相同数值的元素。假设通过排序后发现有两个交换次数都为奇数的元素，并且它们元素数值相等。那么把它们两个交换位置，就可以满足要求。

我们可以通过统计每一个数值它们排序前后出现在奇偶位置的次数是否相等来判断是否符合题目要求。

### 代码
提交时遇到一个玄学bug。明明我在本地使用Microsoft Visual Studio 2017 IDE和编译器写的，样例都对，但是用MS C++ 2017提交到codeforces显示答案错误，选用MS C++ 2010就AC了。
```c++
#include <cstdio>
#include <cstring>
#include <algorithm>

using namespace std;

#define input freopen(R"(C:\Users\Administrator\Desktop\a.txt)", "r", stdin)

const int maxn = 1e5 + 5;

int a[maxn];
int cnt[maxn][2];

int main() {
    //input;
    int t;
    scanf("%d", &t);
    while (t--) {
        int n;
        scanf("%d", &n);
        memset(cnt, 0, sizeof(cnt));
        for (int i = 0; i < n; ++i) {
            scanf("%d", &a[i]);
            ++cnt[a[i]][i % 2];  //统计这个数值出现在奇数和偶数位的次数
        }
        sort(a, a + n);
        for (int i = 0; i < n; ++i) {  //统计这个数排序后出现在奇偶位置的次数
            --cnt[a[i]][i % 2];
        }
        bool flag = true;
        for (int i = 1; i <= maxn; ++i) {  //如果每一个数值在排序前后出现在奇偶位置的次数都相等，则认为它们都交换了偶数次
            for (int j = 0; j < 2; ++j) {
                if (cnt[i][j]) {
                    flag = false;
                    i = maxn + 1;  //退出循环
                }
            }
        }
        if (flag)
            printf("YES\n");
        else
            printf("NO\n");
    }

    return 0;
}
```
