title: 'Codeforces Round #708 (Div. 2)'
author: Monody12
tags:
  - Codeforces
  - modulus
  - greedy
  - lcm
categories:
  - algorithm
date: 2021-04-04 16:46:00
---
## B. M-arrays(贪心 模数)
### 题意
将一个数组中的元素按以下两种条件重组成若干数组
+ 数组中的相邻元素之和是m的倍数
+ 数组中只用一个元素  
求最少需要重组成多少个数组


### 解法

#### 尽量使用数学解法: 避免模拟
模拟(错误解法)
思路: 每次拿出一个存在的数, 然后找出能与其加和为m的数。找的时候不断循环，直到桶为0为止。
错误用例：
```
1
7 6
2 2 2 4 4 4 4
```
如果先模拟到起始点为2则会造成不能充分利用。2，4，2，4，2，4
而如果4作为起点，则可以充分利用。4，2，4，2，4，2，4

```C++
#include <cstdio>

#define input freopen("C:\\Users\\Administrator\\Desktop\\a.txt","r",stdin)
using namespace std;

int a[100005], cnt[100005];

int main() {
    input;
    int t, n, m;
    scanf("%d", &t);
    while (t--) {
        scanf("%d%d", &n, &m);
        for (int i = 0; i < n; ++i) {
            scanf("%d", &a[i]);
            a[i] %= m;
            ++cnt[a[i]]; //桶计数
        }
        // sort(a, a + n);
        int ans = 0;
        for (int i = 0; i < n; ++i) {
            if (cnt[a[i]] <= 0)
                continue;
            int tmp = a[i]; //每次取出序列中的第一个元素
            --cnt[a[i]];
            int dif = (m - tmp) % m;//寻找与其匹配的数
            while (cnt[dif] > 0) {
                --cnt[dif];
                dif = (m - dif) % m;
            }
            ++ans;
        }
        printf("%d\n", ans);
    }
    return 0;
}
```

根据上面的失败经验，我们发现了一个规律。在两个互相匹配的数的个数与生成序列是有规律的。如果两个数的个数绝对值之差小于等于0，那么就只需要一个序列。否则需要多出来的那一个数自己单独成一个序列了。

通过代码：

```C++
#include <cstdio>
#include <cmath>
#include <algorithm>

#define input freopen("C:\\Users\\Administrator\\Desktop\\a.txt","r",stdin)
using namespace std;

int a[100005], cnt[100005];

int main() {
    //input;
    int t, n, m;
    scanf("%d", &t);
    while (t--) {
        scanf("%d%d", &n, &m);
        for (int i = 0; i < n; ++i) {
            scanf("%d", &a[i]);
            a[i] %= m;
            ++cnt[a[i]]; //桶计数
        }
        // sort(a, a + n);
        int ans = 0;
        for (int i = 0; i < n; ++i) {
            if (cnt[a[i]] <= 0) //该数不存在
                continue;
            int a1 = a[i], a2 = (m - a[i]) % m; //本次配对的两个数
            int t1 = cnt[a1], t2 = cnt[a2];
            ans += max(1, abs(t1 - t2)); 
            cnt[a1] = cnt[a2] = 0; //两个数全被用完
        }
        printf("%d\n", ans);
    }
    return 0;
}
```
--------

## C1. k-LCM (easy version) （数学 最小公倍数）

### 题意
将一个整数n分成3个整数，这三个整数满足它们的最小公倍数不大于n/2，并且它们之和等于n。

### 分类讨论
1. 当n为奇数时，拿出一个1，然后对半平分。对半平分可以很好的满足最小公倍数不超过n的一半。1，n/2，n/2
2. 当n为偶数时，并且n/2为奇数。因为n/2为奇数，所以n/2-1必定为偶数，所以答案为。2，n/2-1，n/2-1
3. 当n为偶数时，并且n/2为偶数。那么这个数就是4的倍数。所以可以分成：n/4，n/4，n/2。

很巧妙的一种数学解法，如果模拟的话会超时，我就试过了。。。

```C++
#include <iostream>
#include <algorithm>
#include <cmath>

#define input freopen("C:\\Users\\Administrator\\Desktop\\a.txt","r",stdin)

using namespace std;

int main() {
    //input;
    int t, n, k;
    scanf("%d", &t);
    while (t--) {
        scanf("%d%d", &n, &k);
        if (n % 2 == 1) //n为奇数  3: 1 1 1
            printf("1 %d %d\n", n / 2, n / 2);
        else if (n / 2 % 2 == 1) // n/2为奇数  14: 2 6 6
            printf("2 %d %d\n", n / 2 - 1, n / 2 - 1);
        else  // n/2为偶数  8: 2 2 4
            printf("%d %d %d\n", n / 4, n / 4, n / 2);
    }
    return 0;
}
```