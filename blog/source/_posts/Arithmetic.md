title: 数论
author: Monody12
date: 2021-04-02 11:11:21
tags:
---
### 目录
1. [质数判断](#质数判断)  
  [试除法](#试除法)  
  [线性筛](#线性筛)
2. [分解质因数](#分解质因数)  
3. [最大公因数、最小公倍数](#最大公因数、最小公倍数)
4. [快速幂](#快速幂)
5. [欧拉公式](#欧拉公式)

## 一、<a name="质数判断">质数判断</a>
### <a name="试除法">试除法</a>
```C++
#include <iostream>
#include <cstdio>
#include <cmath>

#define input freopen("C:\\Users\\Administrator\\Desktop\\a.txt","r",stdin)
using namespace std;

bool isprime(int n) {
    if (n < 2)
        return true;
    int x = sqrt(n);
    for (int i = 2; i <= x; ++i)
        if (n % i == 0)
            return false;
    return true;
}

int main() {
    int n, num;
    cin >> n;
    while (n--) {
        scanf("%d", &num);
        if (isprime(num))puts("Yes");
        else puts("No");
    }
    return 0;
}

```

### <a name="线性筛">线性筛</a>
```C++
#include <iostream>
#include <cstdio>
#include <vector>

#define maxn 1000006
using namespace std;
vector<int> primes;
bool isprime[maxn];

void selectPrime(int n) {
    for (int i = 2; i <= n; ++i) {
        if (isprime[i] == 0)
            primes.push_back(i);
        for (int j = 0; primes[j] <= n / i; ++j) {
            isprime[primes[j] * i] = 1;
            if (i % primes[j] == 0)
                break;
        }
    }
}

int main() {
    int n;
    cin >> n;
    selectPrime(n);
    printf("%d", primes.size());
    return 0;
}

```
-----------
## 二、<a name="分解质因数">分解质因数</a>
```C++
#include <iostream>
#include <cstdio>

using namespace std;

void primefactor(int n) {
    for (int i = 2; i <= n / i; ++i) {
        if (n % i == 0) {//找到因子
            int sum = 0;//因子i出现的次数  题中叫指数
            while (n % i == 0) {//循环找出n中含有的i因子
                n /= i;
                ++sum;
            }
            printf("%d %d\n", i, sum);
        }
    }
    if (n > 1)//最后剩下的数为质数
        printf("%d 1\n", n);
}

int main() {
    int n, num;
    cin >> n;
    while (n--) {
        scanf("%d", &num);
        primefactor(num);
        putchar('\n');
    }
    return 0;
}

```

## 三、<a name="最大公因数、最小公倍数">最大公因数、最小公倍数</a>

```C++
#include <iostream>

using namespace std;

int gcd(int a, int b) {//最大公约数
    return b ? gcd(b, a % b) : a;
}

int lcm(int a, int b) {//最小公倍数
    return a * b ? (a * b) / gcd(a, b) : 0;
}

int main() {
    cout << gcd(16, 24) << endl;  //输出8
    cout << lcm(16, 24) << endl;  //输出48
    return 0;
}
```

--------
## 四、<a name="快速幂">快速幂</a>

```C++
#include <iostream>
#include <cstdio>

#define input freopen("C:\\Users\\Administrator\\Desktop\\a.txt","r",stdin)
typedef long long ll;
using namespace std;

int PowerMod(ll a, ll b, ll c) {
    ll ans = 1 % c;
    a %= c;
    while (b) {
        if (b & 1)ans = (ans * a) % c;
        b >>= 1;
        a = (a * a) % c;
    }
    return ans;
}

int main() {
    input;
    int n, i, a, b, p;
    cin >> n;
    while (n--) {
        scanf("%d%d%d", &a, &b, &p);
        printf("%d\n", PowerMod(a, b, p));
    }

    return 0;
}

```
------
## 五、<a name="欧拉公式">欧拉公式</a>
欧拉函数：1∼N 中与 N 互质的数的个数被称为欧拉函数，记为 ϕ(N)。  
以下为求欧拉函数的模板

```C++
#include <iostream>
#include <cstdio>

using namespace std;

int euler(int n) {
    int i, ans = n;
    for (i = 2; i <= n / i; ++i) {
        if (n % i == 0) {
            ans = ans / i * (i - 1);//由欧拉函数公式推出
            while (n % i == 0)//与质因子出现次数无关
                n /= i;
        }
    }
    if (n > 1) ans = ans / n * (n - 1);
    return ans;
}

int main() {
    int n, a;
    cin >> n;
    while (n--) {
        scanf("%d", &a);
        printf("%d\n", euler(a));
    }
    return 0;
}

```