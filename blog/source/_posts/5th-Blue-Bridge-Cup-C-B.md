title: 第五届蓝桥杯大赛软件类省赛 C/C++ 大学B 组
author: Monody12
date: 2021-04-14 18:12:10
tags:
---
<a name="目录">目录</a>
1. [啤酒和饮料](#啤酒和饮料)
1. [切面条](#切面条)
1. [李白打酒](#李白打酒)
1. [史丰收速算](#史丰收速算)
1. [打印图形](#打印图形)
1. [奇怪的分式](#奇怪的分式)
1. [六角填数](#六角填数)
1. [蚂蚁感冒](#蚂蚁感冒)
1. [地宫取宝](#地宫取宝)
1. [小朋友排队](#小朋友排队)


## <a name="啤酒和饮料">啤酒和饮料 (枚举)</a>

啤酒每罐2.3元，饮料每罐1.9元。小明买了若干啤酒和饮料，一共花了82.3元。

我们还知道他买的啤酒比饮料的数量少，请你计算他买了几罐啤酒。

### 分析
数据范围不大: 直接枚举啤酒和饮料的数量即可

```c++
#include <cstdio>

using namespace std;
#define input freopen("C:\\Users\\Administrator\\Desktop\\a.txt","r",stdin)

int main() {
    //input;
    for (int i = 0; i < 100; ++i)   //枚举啤酒数量
        for (int j = i + 1; j < 100; ++j)  //枚举饮料数量
            if (i * 2.3 + j * 1.9 == 82.3)
                printf("啤酒买了%d罐 饮料买了%d罐", i, j);
    return 0;
}
```

输出答案：啤酒买了11罐 饮料买了30罐  
填入答案：11

[回到顶部](#目录)

--------------
## <a name="切面条">切面条</a>

一根高筋拉面，中间切一刀，可以得到2根面条。

如果先对折1次，中间切一刀，可以得到3根面条。

如果连续对折2次，中间切一刀，可以得到5根面条。

那么，连续对折10次，中间切一刀，会得到多少面条呢？

### 分析
参加一位大神的[解析](https://blog.csdn.net/weixin_37590454/article/details/83684664)  
我认为用纸来代替面条真的是一个绝妙的操作。省去了抽象的思考。实验出对折3次后中间切一刀可以得到9根面条。  
总结一下： 

| 对折次数 n| 得到的面条数 f(n) |
| :-------:| :-----------:|
|0|2|
|1|3|
|2|5|
|3|9|
|n|2 \* f(n-1) - 1|

```c++
#include <cstdio>

using namespace std;
#define input freopen("C:\\Users\\Administrator\\Desktop\\a.txt","r",stdin)

int main() {
    //input;
    int f[11] = {2, 3, 5, 9};
    for (int i = 4; i <= 10; ++i) {
        f[i] = 2 * f[i - 1] - 1;
    }
    printf("%d", f[10]);
    return 0;
}
```

输出答案：1025

[回到顶部](#目录)

--------------
## <a name="李白打酒">李白打酒 （dfs）</a>

话说大诗人李白，一生好饮。幸好他从不开车。

一天，他提着酒壶，从家里出来，酒壶中有酒2斗。他边走边唱：

无事街上走，提壶去打酒。
逢店加一倍，遇花喝一斗。

这一路上，他一共遇到店5次，遇到花10次，已知最后一次遇到的是花，他正好把酒喝光了。 

请你计算李白遇到店和花的次序，可以把遇店记为a，遇花记为b。则：babaabbabbabbbb 就是合理的次序。像这样的答案一共有多少呢？请你计算出所有可能方案的个数（包含题目给出的）。

### 分析
递归法尝试李白每次是遇到花还是店。


```c++
#include <cstdio>

using namespace std;
#define input freopen("C:\\Users\\Administrator\\Desktop\\a.txt","r",stdin)

/**
 * 递归深搜李白喝酒的方案
 * @param alcohol 当前酒量
 * @param flower 剩余遇到的花的数量
 * @param shop 剩余遇到的店的数量
 * @param last_meet 上一次遇到的是花还是店 花是1 店是2
 * @param ans 当前可行的方案数
 */
void dfs(int alcohol, int flower, int shop, int last_meet, int &ans) {
    if (alcohol == 0) {
        if (flower == 0 && shop == 0 && last_meet == 1)
            ++ans;
        return;
    }
    if (flower > 0)
        dfs(alcohol - 1, flower - 1, shop, 1, ans);  //假设遇到花了
    if (shop > 0)
        dfs(alcohol * 2, flower, shop - 1, 2, ans);  //假设遇到店了
}

int main() {
    //input;
    int ans = 0;
    dfs(2, 10, 5, 0, ans);
    printf("%d", ans);
    return 0;
}
```
输出答案：14

[回到顶部](#目录)

--------------
## <a name="史丰收速算">史丰收速算</a>

[回到顶部](#目录)

--------------
## <a name="打印图形">打印图形</a>

[回到顶部](#目录)

--------------
## <a name="奇怪的分式">奇怪的分式 （枚举）</a>

上小学的时候，小明经常自己发明新算法。一次，老师出的题目是：
1/4 乘以 8/5   
小明居然把分子拼接在一起，分母拼接在一起，答案是：18/45 （参见图1.png）  
老师刚想批评他，转念一想，这个答案凑巧也对啊，真是见鬼！  
对于分子、分母都是 1~9 中的一位数的情况，还有哪些算式可以这样计算呢？  
请写出所有不同算式的个数（包括题中举例的）。  
显然，交换分子分母后，例如：4/1 乘以 5/8 是满足要求的，这算做不同的算式。  
但对于分子分母相同的情况，2/2 乘以 3/3 这样的类型太多了，不在计数之列!  

![奇怪的分式题目.PNG](https://i.loli.net/2021/04/14/AOnPSJ9wy2ms6Mu.png)

注意：答案是个整数（考虑对称性，肯定是偶数）。请通过浏览器提交。不要书写多余的内容。

### 分析
枚举这四个数再检查表达式是否成立即可。

```c++
#include <cstdio>
#include <cmath>

using namespace std;
#define input freopen("C:\\Users\\Administrator\\Desktop\\a.txt","r",stdin)

bool check(double i, double j, double k, double l) {  //检查表达式是否正确
    double left = i / j * k / l;
    double right = (i * 10 + k) / (j * 10 + l);
    return fabs(left - right) <= 1e-5;
}

int main() {
    int ans = 0;
    for (int i = 1; i < 10; ++i)
        for (int j = 1; j < 10; ++j)
            for (int k = 1; k < 10; ++k)
                for (int l = 1; l < 10; ++l) {
                    if (i == j && k == l)  //消除两对分子分母都相同的情况
                        continue;
                    if (check(i, j, k, l))
                        ++ans;
                }
    printf("%d", ans);
    return 0;
}

```

[回到顶部](#目录)

--------------
## <a name="六角填数">六角填数</a>

如图【1.png】所示六角形中，填入1~12的数字。

使得每条直线上的数字之和都相同。

图中，已经替你填好了3个数字，请你计算星号位置所代表的数字是多少？

![六角填数题目.png](https://i.loli.net/2021/04/15/rHdhFzxbiCPnI3M.png)

### 分析
全排列把剩余的孔都进行填充，寻找正确的解法。
实现：
将剩余的孔看出未知量，我是这样设未知量的。
![六角填数写法.jpg](https://i.loli.net/2021/04/15/kKBc5sx78mYyjvw.jpg)

```c++
#include <cstdio>
#include <cmath>

using namespace std;
#define input freopen("C:\\Users\\Administrator\\Desktop\\a.txt","r",stdin)

int a[9], vis[13];

bool check() {  //检查当前分布是否满足每行之和相等
    int line[7] = {0};
    line[1] = 8 + a[0] + a[1] + a[2];
    line[2] = 1 + a[0] + a[3] + a[5];
    line[3] = 1 + a[1] + a[4] + a[8];
    line[4] = a[2] + a[4] + a[7] + 3;
    line[5] = 8 + a[3] + a[6] + 3;
    line[6] = a[5] + a[6] + a[7] + a[8];
    for (int i = 1; i <= 6; ++i)
        for (int j = 1; j <= 6; ++j)
            if (line[i] != line[j])
                return false;
    for (int i = 0; i <= 8; ++i) {
        printf("%d ", a[i]);
    }
    printf("\n");
    return true;
}

void dfs(int step, int n) {  //全排列枚举每个空位出现的数字
    if (step > n) {
        check();
        return;
    }
    for (int i = 1; i <= 12; ++i) {
        if (!vis[i]) {
            vis[i] = 1;
            a[step] = i;
            dfs(step + 1, n);
            vis[i] = 0;
        }
    }
}

int main() {
    vis[8] = vis[3] = vis[1] = 1;  //已固定的值
    dfs(0, 8);
    return 0;
}
```

输出答案：9 2 7 10 12 6 5 4 11  
填入答案：10

[回到顶部](#目录)

--------------
## <a name="蚂蚁感冒">蚂蚁感冒</a>

[回到顶部](#目录)

--------------
## <a name="地宫取宝">地宫取宝</a>

[回到顶部](#目录)

--------------
## <a name="小朋友排队">小朋友排队</a>

[回到顶部](#目录)

--------------










