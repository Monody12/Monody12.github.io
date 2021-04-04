title: '背包问题 '
author: Monody12
tags:
  - algorithm
  - knapsack problem
categories:
  - algorithm
date: 2021-04-01 21:21:00
---
## 目录
1. [引入](#引入)
2. [01背包](#01背包)
3. [完全背包](#完全背包)
4. [多重背包](#多重背包)
5. [分组背包](#分组背包)
6. [二维费用的背包问题](#二维费用的背包问题) 未更新
7. [背包的实际应用](#背包的实际应用) 未更新

### 一、<a name="引入">引入</a>
为什么会有背包问题？  
答：

### 二、<a name="01背包">01背包</a>
#### 每种物品有且仅有一个

01背包模板题 
![01背包问题题目.jpg](https://i.loli.net/2021/04/01/2BtG3HEOvbgy6iJ.jpg)
输入样例  
```
4 5
1 2  
2 4  
3 4  
4 5  
```
输出样例：  
```
8
```
01背包应用了动态规划思想
#### 二维存储朴素做法 :
一重循环: 从第一件物品开始选,每次多考虑选一件
二重循环: 从使用了0个单位体积到完全使用背包体积进行考虑


```C++
#include <iostream>
#include <algorithm>

#define maxn 1006
using namespace std;
int v[maxn], w[maxn], dp[maxn][maxn];

int main() {

    int n, volume;
    scanf("%d%d", &n, &volume);
    for (int i = 1; i <= n; ++i) {
        scanf("%d%d", v + i, w + i);
    }
    for (int i = 1; i <= n; ++i) {
        for (int j = 0; j <= volume; ++j) {
            dp[i][j] = dp[i - 1][j];
            if (j >= v[i])
                dp[i][j] = max(dp[i][j], dp[i - 1][j - v[i]] + w[i]);
        }
    }
    printf("%d", dp[n][volume]);
    return 0;
}

```
#### <a name="优化进阶">优化进阶</a>
动态规划数组 **滚动数组 优化成一维**
观察状态转移方程 dp[i][j] = max(dp[i][j], dp[i - 1][j - v[i]] + w[i]);
发现当选前第i件物品的选择方案依赖选第i-1件物品的方案, 但是选第i件时, 只有**选前第i-1件物品最终的方案**会被更新至第i件。
选的过程dp[i-1][0]~dp[i-1][volume]将毫无意义，造成内存浪费
所以可以考虑使用一维数组来存放dp的过程。
注意：在去掉第一维时，由于状态转移方程中dp[i - 1][j - v[i]] + w[i]是基于前i-1个物品的选择方案，而去掉后则变为前第i个物品的方案，如果继续从背包小容量到满容量去递推，则会造成**同一个物品被多次使用**。所以枚举使用的背包容量时应当递减
```C++
#include <iostream>
#include <algorithm>

#define input 
#define maxn 1006
using namespace std;
int v[maxn], w[maxn], dp[maxn];

int main() {
    int n, volume;
    scanf("%d%d", &n, &volume);
    for (int i = 0; i < n; ++i) {
        scanf("%d%d", v + i, w + i);
    }
    for (int i = 0; i < n; ++i) {
        for (int j = volume; j >= v[i]; --j) {
            dp[j] = max(dp[j], dp[j - v[i]] + w[i]);
        }
    }
    printf("%d",dp[volume]);
    return 0;
}

```

--------
### 三、<a name="完全背包">完全背包</a>
完全背包模板题  
![多重背包_需优化_题目.jpg](https://i.loli.net/2021/04/01/q3O2fuejTkJ5cUw.jpg)
输入样例
```
4 5
1 2
2 4
3 4
4 5
```
输出样例：
```
10
```
#### 与01背包的区别：每样物品从只能使用一次变为可以使用无限次  
正是因为只有这个区别和特点：所以将01背包模板中体积循环从大到小**改为从小到大**递推即可实现一件物品可以多次被用到。（为什么会被多次用到请阅读01背包的[优化进阶](#优化进阶)部分）  


```C++
#include <iostream>
#include <algorithm>

#define input freopen("C:\\Users\\Administrator\\Desktop\\a.txt","r",stdin)
#define maxn 1006
using namespace std;
int v[maxn], w[maxn], dp[maxn];

int main() {

    //input;
    int n, volume;
    scanf("%d%d", &n, &volume);
    for (int i = 0; i < n; ++i) {
        scanf("%d%d", v + i, w + i);
    }
    for (int i = 0; i < n; ++i) {
        for (int j = v[i]; j <= volume; ++j) {
            dp[j] = max(dp[j], dp[j - v[i]] + w[i]);
        }
    }
    printf("%d", dp[volume]);
    return 0;
}

```
-------
### 四、<a name="多重背包">多重背包</a>
#### 多重背包 （朴素版）

多重背包（朴素版）模板题
![多重背包_无需优化_题目.jpg](https://i.loli.net/2021/04/01/ZPkqmeipGrzLhKV.jpg)
输入样例
```
4 5
1 2 3
2 4 1
3 4 3
4 5 2
```
输出样例：
```
10
```
与01背包的区别：每件物品最多可以选择指定的次数  
那么我们就在01背包的基础上**增加一重循环**，来枚举应该在**指定的次数中选择多少次**比较优

```C++
#include <iostream>
#include <algorithm>

#define input freopen("C:\\Users\\Administrator\\Desktop\\a.txt","r",stdin)
#define maxn 1006
using namespace std;
int v[maxn], w[maxn], s[maxn], dp[maxn];

int main() {

    //input;
    int n, volume;
    scanf("%d%d", &n, &volume);
    for (int i = 1; i <= n; ++i) {
        scanf("%d%d%d", v + i, w + i, s + i);
    }
    for (int i = 1; i <= n; ++i) {
        for (int j = volume; j >= v[i]; --j) {
            for (int k = 1; k <= s[i] && j >= k * v[i]; ++k) {
                dp[j] = max(dp[j], dp[j - k * v[i]] + k * w[i]);
            }
        }
    }
    printf("%d", dp[volume]);
    return 0;
}

```

#### 多重背包问题 （二进制、倍增优化版）
多重背包（优化版）模板题
![多重背包_需优化_题目.jpg](https://i.loli.net/2021/04/01/q3O2fuejTkJ5cUw.jpg)
输入样例
```
4 5
1 2 3
2 4 1
3 4 3
4 5 2
```
输出样例：
```
10
```

如果像朴素做法一样来一个一个地枚举，时间复杂度会很高  
我们就将同一类的物品按照**二进制倍增**来**打包**成log2 s[i] + 1堆，再**转换为01背包**即可

```C++
#include <iostream>
#include <algorithm>
#include <vector>
#include <cmath>

#define input freopen("C:\\Users\\Administrator\\Desktop\\a.txt","r",stdin)
#define maxn 2006
using namespace std;
int v[maxn], w[maxn], s[maxn], dp[maxn];
vector<int>packetW, packetV; //用于将多件物品按找2的倍数来打包;
int main() {

	//input;
	int n, volume;
	scanf("%d%d", &n, &volume);
	for (int i = 1, j; i <= n; ++i) {
		scanf("%d%d%d", v + i, w + i, s + i);
		for (j = 1; j <= s[i]; s[i] -= j, j *= 2) {
			packetV.push_back(j*v[i]);
			packetW.push_back(j*w[i]);
		}
		if (s[i]) {//按2的倍数打包所剩的物品
			packetW.push_back(s[i]*w[i]);
			packetV.push_back(s[i]*v[i]);
		}
	}
	for (int i = 0; i < packetW.size(); ++i) {
		for (int j = volume; j >= packetV[i]; --j) {
			dp[j] = max(dp[j], dp[j - packetV[i]] + packetW[i]);
		}
	}
	printf("%d", dp[volume]);
	return 0;
}

```
### 五、分组背包
分组背包模板题
![分组背包问题题目.jpg](https://i.loli.net/2021/04/01/uFznRCNdEHaPJ3f.jpg)
输入样例
```
3 5
2
1 2
2 4
1
3 4
1
4 5
```
输出样例：
```
8
```

代码:
```C++
#include <iostream>
#include <algorithm>

using namespace std;

const int N = 110;

int n, m;
int v[N][N], w[N][N], s[N];
int f[N];

int main()
{
    cin >> n >> m;
    for (int i = 1; i <= n; i ++ ){
        cin >> s[i];
        for (int j = 0; j < s[i]; j ++ )
            cin >> v[i][j] >> w[i][j];
    }
    for (int i = 1; i <= n; i ++ )
        for (int j = m; j >= 0; j -- )
            for (int k = 0; k < s[i]; k ++ )
                if (v[i][k] <= j)
                    f[j] = max(f[j], f[j - v[i][k]] + w[i][k]);
    cout << f[m] << endl;
    return 0;
}

```

