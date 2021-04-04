title: 'Codeforces Round #710 (Div. 3)  A-D'
author: Monody12
tags:
  - Codeforces
  - algorithm
categories:
  - Codeforces
date: 2021-04-01 08:15:00
---
[原题传送门](https://codeforces.com/contest/1506)
## [A. Strange Table (数学)](https://codeforces.com/contest/1506/problem/A)
### 题意
给出一个按列排序（同一列上的每行递增1）的矩阵（n行m列）和这个矩阵上的一个数（求出这个数的横纵坐标）。要求你给出成按行排序后的矩阵这个横纵坐标上对应的数。
如何求矩阵的横纵坐标，如下图所示。
![矩阵问题技巧.jpg](https://i.loli.net/2021/04/01/LGKfebwVpTqr1WM.jpg)
（示例3）拿一个3行5列的按列排序的矩阵来说：要求11这个数的坐标。根据每3个数一列这个性质。我们可能用11/3来得到列坐标，11%3来得到行坐标。坐标均从0开始计算。转换为行矩阵也可以用这个类似的性质，读者不妨自己思考一下。
```C++
#include <cstdio>
 
using namespace std;
 
int main() {
    int t, n, m;
    long long x;
    scanf("%d", &t);
    while (t--) {
        scanf("%d%d%lld", &n, &m, &x);
        x--; //起始坐标值减一便于计算
        long long line = x % n; //计算按列展开的行列数
        long long col = x / n;
        long long ans = line * m + col + 1; //别忘了把少算的1加回来
        printf("%lld\n", ans);
    }
    return 0;
}
```

--------------
## B. [Partial Replacement (贪心)](https://codeforces.com/contest/1506/problem/B)
### 题意
有一串包含 '\*' 和 '.' 的字符串。需要对这个字符串做以下操作。  
+  将首个 '\*' 替换为 'x'
+ 将最后一个 '\*' 替换为 'x'
+ 相邻两个 'x' 的距离不能超过k个字符  
在满足以上条件的前提下，求需要替换的最少次数。  
贪心策略：第一个 '\*' 必定会替换为 'x'，记住这个位置。在满足与第三个条件的情况下，两个替换后的'x'离的越远，那么替换的次数就越少（局部最优解）。

```C++
#include <cstdio>
#include <iostream>
#include <algorithm>

using namespace std;

int main() {
    int t, n, k;
    scanf("%d", &t);
    while (t--) {
        scanf("%d%d", &n, &k);
        char s[60], backup[60];
        cin >> s;
        memcpy(backup, s, sizeof(s));
        int first = -1, last = -1;//第一个和最后一个*的下标
        for (int i = 0; i < n; ++i) { //求首位和末尾*的下标
            if (s[i] == '*') {
                if (first == -1)
                    first = i;
                last = i;
            }
        }
        if (first == last) {  //整个字符串只有一个*
            puts("1");
            continue;
        }
        s[first] = 'x';
        int l = first; //头指针
        int r; //记录最后一个有效的*位置
        for (int i = first + 1; i < n; ++i) {
            if (i - l > k) { //当前距离已经超过k了
                s[r] = 'x';
                l = r;
            }
            if (s[i] == '*') {
                if (i - l <= k) {
                    r = i;
                }
            }
        }
        if (s[last] == '*') {  //如果最后一个*没有被替换，手动替换
            s[last] = 'x';
        }
        int ans = 0;
        for (int i = 0; i < n; ++i) { //计算与原字符串相比替换了多少个*
            if (backup[i] != s[i])
                ++ans;
        }
        printf("%d\n", ans);
    }
    return 0;
}

```

-----------
## [C. Double-ended Strings (枚举 字符串匹配)](https://codeforces.com/contest/1506/problem/C)
### 题意
寻找给出的两个字符串最大的重复子串a和b。
题目的数据规模很小，暴力枚举出a字符串所有可能出现的子串（利用C++中的string-substr），然后寻找b中是否出现过（string-find函数）。
注意：s.substr(index,len)的用法是：从字符串下标index处开始向右截取len个字符。

```C++
#include <cstdio>
#include <string>
#include <iostream>
#include <cstring>
#include <algorithm>

using namespace std;

int main() {
    int t;
    cin >> t;
    while (t--) {
        string a, b;
        cin >> a >> b;
        if (a == b) {
            puts("0");
            continue;
        }
        int len_a = a.length(), len_b = b.length();
        int ans = 0;
        for (int i = 0; i < len_a; ++i) {
            for (int j = 1; j <= len_a - i; ++j) {
               // cout<<"出现的子串"<<a.substr(i, j)<<"  ";
                if (b.find(a.substr(i, j)) != string::npos) {
                    ans = max(ans, (int) a.substr(i, j).length()); //找最长公共子串
                   // puts("字符串匹配");
                }
//                else
//                    puts("字符串不匹配");
            }
        }
       // printf("子串长度 %d\n",ans);
        printf("%d\n", len_a + len_b - 2 * ans);
    }

    return 0;
}

```
---------
## D. [Epic Transformation （贪心）](https://codeforces.com/contest/1506/problem/D)
### 题意
给出一个数组，数组中数值不同的两个数可以两两相消。求这个数组中最少可以留下多少个数。
根据题意：我们可以将数值相同的数进行归类，由于值域很大，需要使用map。
两两相消的过程也是需要按一定的原则的。

例如：1，1，2，2，3，3  
不能简单的拿两次1,2就消掉，这样就会剩下两个3 。其实还有更优的方法，就是1，2；2，3；1，3这样就可以把全部的数给消掉了。但是，如果重复的数过多，自然是没有办法消完的，例如1，1，2，2，2，2，2，2 。这时，我们就需要分析一下，当重复次数最多的一个数，多到什么情况下，就会使数组中的数不能被全部消去。简单想一下，便是占总数的1/2时可以恰好与其他值不同的数给消完。
```C++
#include <cstdio>
#include <climits>
#include <unordered_map>

using namespace std;

int main() {
    int t, n, x;
    scanf_s("%d", &t);
    while (t--) {
        unordered_map<int, int> map;
        scanf_s("%d", &n);
        for (int i = 0; i < n; ++i) {
            scanf_s("%d", &x);
            ++map[x];
        }
        int max_cnt = INT_MIN; //统计出现次数最多的一个数
        for (auto &i:map)
            if (i.second > max_cnt)
                max_cnt = i.second;
        if (max_cnt <= n / 2) { //如果出现最多的一个数小于等于总数个数的一半,则可以两两相消
            if (n % 2 == 1)
                puts("1");
            else puts("0");
        } else { //每次用出现最多的数去和别的数两两相消
            printf("%d\n", max_cnt - (n - max_cnt));
        }
    }
    return 0;
}

```
