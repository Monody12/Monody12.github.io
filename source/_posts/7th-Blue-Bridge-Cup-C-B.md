title: 第七届蓝桥杯大赛软件类省赛 C/C++ 大学B 组
author: Monody12
tags:
  - Blue Bridge Cup
  - quick sort
  - enumeration
  - permutation
  - hash
  - greedy
categories:
  - algorithm
date: 2021-04-10 12:27:00
---
## <a name="目录">目录</a>
1. [煤球数目](#煤球数目)
2. [生日蜡烛](#生日蜡烛)
3. [凑算式](#凑算式)
4. [快速排序](#快速排序)
5. [抽签](#抽签)
6. [方格填数](#方格填数)
7. [剪邮票](#剪邮票)
8. [四平方和](#四平方和)
9. [交换瓶子](#交换瓶子)
10. [最大比例](#最大比例)

## <a name="煤球数目">1.煤球数目 （枚举）</a>
煤球数目  

有一堆煤球，堆成三角棱锥形。具体：  
第一层放1个，  
第二层3个（排列成三角形），  
第三层6个（排列成三角形），  
第四层10个（排列成三角形），  
....  
如果一共有100层，共有多少个煤球？  

请填表示煤球总数目的数字。  
注意：你提交的应该是一个整数，不要填写任何多余的内容或说明性文字。  

### 分析
应该很容易看出规律：每一层比上一层多x个煤球，x为当前层的层数。  
枚举一下这100层的煤球数量累加即可。

```c++
#include <cstdio>

#define input freopen("C:\\Users\\Administrator\\Desktop\\a.txt","r",stdin)

using namespace std;

int main() {
    //input;
    int sum = 0, cnt = 0;
    for (int i = 1; i <= 100; ++i) {
        cnt += i;
        sum += cnt;
    }
    printf("%d", sum);
    return 0;
}
```

输出答案：171700

[回到顶部](#目录)

------------
## <a name="生日蜡烛">2.生日蜡烛 （枚举）</a>

某君从某年开始每年都举办一次生日party，并且每次都要吹熄与年龄相同根数的蜡烛。

现在算起来，他一共吹熄了236根蜡烛。

请问，他从多少岁开始过生日party的？

请填写他开始过生日party的年龄数。
注意：你提交的应该是一个整数，不要填写任何多余的内容或说明性文字。

### 分析
因为这吹蜡烛的数量是一个等差数列，所以枚举开始举办的年龄和当前年龄即可。

```c++
#include <cstdio>

#define input freopen("C:\\Users\\Administrator\\Desktop\\a.txt","r",stdin)

using namespace std;

int f(int start, int end) { //等差数列求和公式 (公差为1)
    int n = end - start + 1;
    return (start + end) * n / 2;
}

int main() {
    //input;
    int sum = 0, cnt = 0;
    for (int i = 1; i <= 100; ++i) {
        cnt += i;
        sum += cnt;
    }
    for (int i = 0; i < 236; ++i)  //枚举起始和终点
        for (int j = i + 1; j < 236; ++j)
            if (f(i, j) == 236)
                printf("start:%d end:%d", i, j);
    return 0;
}
```

输出答案：start:26 end:33  
填入答案：26

[回到顶部](#目录)

------------
## <a name="凑算式">3.凑算式 （全排列 枚举）</a>

![凑算式题目.jpg](https://i.loli.net/2021/04/10/3faTRYZwMnzrl1F.jpg)
	 
	 
这个算式中A~I代表1~9的数字，不同的字母代表不同的数字。

比如：  
6+8/3+952/714 就是一种解法，  
5+3/1+972/486 是另一种解法。

这个算式一共有多少种解法？

注意：你提交应该是个整数，不要填写任何多余的内容或说明性文字。

### 分析
题意：求出满足这个表达式的ABCDEFGHI这十个未知数分别对应1-9这九个数(不能重复)的解法数量。  
实现：全排列枚举。

```C++
#include <cstdio>
#include <cmath>
#include <algorithm>

#define input freopen("C:\\Users\\Administrator\\Desktop\\a.txt","r",stdin)

using namespace std;

double a[] = {1, 2, 3, 4, 5, 6, 7, 8, 9};
double &A = a[0], &B = a[1], &C = a[2], &D = a[3], &E = a[4], &F = a[5], &G = a[6], &H = a[7], &I = a[8];

bool check() { //检查是否满足表达式
    double ans = A + B / C + (100 * D + 10 * E + F) / (100 * G + 10 * H + I) - 10;
    if (fabs(ans) < 1e-7)
        return true;
    else return false;
}

int main() {
    //input;
    int ans = 0;
    do {
        if (check())
            ++ans;
    } while (next_permutation(a, a + 9));
    printf("%d", ans);
    return 0;
}
```

[回到顶部](#目录)

------------
## <a name="快速排序">4.快速排序</a>

排序在各种场合经常被用到。
快速排序是十分常用的高效率的算法。

其思想是：先选一个“标尺”，
用它把整个队列过一遍筛子，
以保证：其左边的元素都不大于它，其右边的元素都不小于它。

这样，排序问题就被分割为两个子区间。
再分别对子区间排序就可以了。

下面的代码是一种实现，请分析并填写划线部分缺少的代码。

```c
#include <stdio.h>

void swap(int a[], int i, int j)
{
	int t = a[i];
	a[i] = a[j];
	a[j] = t;
}

int partition(int a[], int p, int r)
{
    int i = p;
    int j = r + 1;
    int x = a[p];
    while(1){
        while(i<r && a[++i]<x);
        while(a[--j]>x);
        if(i>=j) break;
        swap(a,i,j);
    }
	______________________;
    return j;
}

void quicksort(int a[], int p, int r)
{
    if(p<r){
        int q = partition(a,p,r);
        quicksort(a,p,q-1);
        quicksort(a,q+1,r);
    }
}
    
int main()
{
	int i;
	int a[] = {5,13,6,24,2,8,19,27,6,12,1,17};
	int N = 12;
	
	quicksort(a, 0, N-1);
	
	for(i=0; i<N; i++) printf("%d ", a[i]);
	printf("\n");
	
	return 0;
}
```

注意：只填写缺少的内容，不要书写任何题面已有代码或说明性文字。

[回到顶部](#目录)

------------
## <a name="抽签">5.抽签</a>

[回到顶部](#目录)

------------
## <a name="方格填数">6.方格填数 （排列 枚举）</a>

如下的10个格子
![方格填数题目.jpg](https://i.loli.net/2021/04/11/iCT2wxzV8SbHsMU.jpg)

（如果显示有问题，也可以参看【图1.jpg】）

填入0~9的数字。要求：连续的两个数字不能相邻。
（左右、上下、对角都算相邻）

一共有多少种可能的填数方案？

请填写表示方案数目的整数。
注意：你提交的应该是一个整数，不要填写任何多余的内容或说明性文字。

### 分析
题意：在给定的方格中填入十个数字，每个数字相邻的格子（8个方向）上不能是相邻的数字（绝对值之差不能为1），求合法的填入方案。  
实现：全排列枚举这十个方格对应的0-9这十个数字。然后依次检查每个格子的合法性。
疑点：题中没有规定填入方格的数字不能重复，以下代码是以不重复来写的。

```c++
#include <cstdio>
#include <algorithm>

using namespace std;
int a[] = {0, 1, 2, 3, 4, 5, 6, 7, 8, 9};
int map[3][4];
int dr[][2] = {{1,  0},
               {0,  1},
               {-1, 0},
               {0,  -1},
               {1,  1},
               {-1, 1},
               {1,  -1},
               {-1, -1}};

void init() {
    int index = 0;
    for (int i = 1; i <= 3; ++i)
        map[0][i] = a[index++];
    for (int i = 0; i < 4; ++i)
        map[1][i] = a[index++];
    for (int i = 0; i < 3; ++i)
        map[2][i] = a[index++];
}

bool check(int x, int y) {
    int val = map[x][y];
    if (val == 999)
        return true;
    for (int i = 0; i < 8; ++i) {
        int dx = x + dr[i][0], dy = y + dr[i][1];
        if (dx < 0 || dy < 0 || dx > 2 || dy > 3)
            continue;
        else if (abs(map[dx][dy] - val) == 1)
            return false;
    }
    return true;
}

int main() {
    int ans = 0;
    map[0][0] = map[2][3] = 999;
    do {
        init();
        bool flag = true;
        for (int i = 0; i < 3; ++i)
            for (int j = 0; j < 4; ++j)
                if (!check(i, j)) {
                    i = 3; //用于退出循环
                    flag = false;
                    break;
                }
        if (flag)
            ++ans;
    } while (next_permutation(a, a + 10));
    printf("%d", ans);
    return 0;
}

```
输出答案：1580

[回到顶部](#目录)

------------
## <a name="剪邮票">7.剪邮票</a>



[回到顶部](#目录)

------------
## <a name="四平方和">8.四平方和 （枚举 哈希）</a>

四平方和定理，又称为拉格朗日定理：  
每个正整数都可以表示为至多4个正整数的平方和。  
如果把0包括进去，就正好可以表示为4个数的平方和。

比如：  
5 = 0^2 + 0^2 + 1^2 + 2^2  
7 = 1^2 + 1^2 + 1^2 + 2^2  
（^符号表示乘方的意思）

对于一个给定的正整数，可能存在多种平方和的表示法。  
要求你对4个数排序：  
0 <= a <= b <= c <= d  
并对所有的可能表示法按 a,b,c,d 为联合主键升序排列，最后输出第一个表示法


程序输入为一个正整数N (N<5000000)  
要求输出4个非负整数，按从小到大排序，中间用空格分开

例如，输入：  
5  
则程序应该输出：  
0 0 1 2

再例如，输入：  
12  
则程序应该输出：  
0 2 2 2

再例如，输入：  
773535  
则程序应该输出：  
1 1 267 838

资源约定：  
峰值内存消耗 < 256M  
CPU消耗  < 3000ms

请严格按要求输出，不要画蛇添足地打印类似：“请您输入...” 的多余内容。

所有代码放在同一个源文件中，调试通过后，拷贝提交该源码。

### 分析 
题意：将给出的任意一个数分成四个数的平方和，还要满足a≤b≤c≤d。  
实现：我们可以采用枚举变量的方式从小到大枚举，这样就能保证满足这个条件。可以暴力的使用三层for循环去枚举abc，然后用减法来求出d。但是这样我觉得时间复杂度可能太高了，怕超时。于是我选择了只枚举两个变量。  
首先将cd能够凑成的平方和存起来，为了方便在枚举ab的时候查询，选用**数组哈希**的方式将平方和映射到数组下标。枚举cd平方和的时候如果出现重复的值时**只存入第一个cd变量**，因为枚举时是按照字典序的，题目也要求我们要按照字典序。因此按字典序枚举ab时出来的第一个答案就是我们要找的答案。

```c++
#include <cstdio>

using namespace std;
#define input freopen("C:\\Users\\Administrator\\Desktop\\a.txt","r",stdin)

const int maxn = 7e6 + 2;
struct a {
    int c = -1, d;  //存储平方和为sum[]时c和d的值
} sum[maxn];

int main() {
    //input;
    int n;
    scanf("%d", &n);
    for (int c = 0; c * c <= n; ++c)  //枚举c和d可能出现的平方和组成
        for (int d = 0; c * c + d * d <= n; ++d) {
            int square = c * c + d * d;
            if (sum[square].c == -1) //按字典序枚举 所以只需保留第一个
                sum[square].c = c, sum[square].d = d;
        }
    for (int a = 0; a * a <= n; ++a) {  //枚举ab的同时寻找与之对应的 cd对 
    数组哈希查找
        for (int b = 0; b * b + a * a <= n; ++b) {
            int val = n - a * a - b * b;
            if (sum[val].c != -1) {
                printf("%d %d %d %d", a, b, sum[val].c, sum[val].d);
                exit(0);
            }
        }
    }
    return 0;
}
```

[回到顶部](#目录)

------------
## <a name="交换瓶子">9.交换瓶子 (贪心)</a>
有N个瓶子，编号 1 ~ N，放在架子上。

比如有5个瓶子：
2 1 3 5 4

要求每次拿起2个瓶子，交换它们的位置。
经过若干次后，使得瓶子的序号为：
1 2 3 4 5

对于这么简单的情况，显然，至少需要交换2次就可以复位。

如果瓶子更多呢？你可以通过编程来解决。

输入格式为两行：
第一行: 一个正整数N（N<10000）, 表示瓶子的数目
第二行：N个正整数，用空格分开，表示瓶子目前的排列情况。

输出数据为一行一个正整数，表示至少交换多少次，才能完成排序。

例如，输入：  
5  
3 1 2 5 4

程序应该输出：
3

再例如，输入：  
5  
5 4 3 2 1

程序应该输出：  
2

资源约定：  
峰值内存消耗 < 256M  
CPU消耗  < 1000ms

### 分析
每次交换必然可以使至少1个瓶子还原成原位，所以交换次数不会超过n次。  
O(n<sup>2</sup>)从头开始还原，发现瓶子不在原位就搜索整个瓶子区域即可。n<=10000 853ms 。方法通俗简单，考试勉强能过。
[//]: # (打卡模板，上面预览按钮可以展示预览效果 ^^)
```C++
#include <cstdio>
#include <algorithm>

using namespace std;

int e[10001], n;

int main() {
    scanf("%d", &n);
    for (int i = 1; i <= n; ++i) {
        scanf("%d", e + i);
    }
    int cnt = 0; //需要交换的次数
    for (int i = 1; i <= n; ++i) {
        if (e[i] != i) { //如果该位置上的数与预期不一致, 就在所有数中找到它
            for (int j = 1; j <= n; ++j) { //遍历所有数
                if (e[j] == i) { //找到了
                    swap(e[j], e[i]); //交换值
                    ++cnt;
                    break;
                }
            }
        }
    }
    printf("%d", cnt);
    return 0;
}

```


[回到顶部](#目录)

------------
## <a name="最大比例">10.最大比例</a>

[回到顶部](#目录)

------------