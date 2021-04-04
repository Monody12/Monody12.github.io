title: 区间选点 （贪心）
author: Monody12
tags:
  - algorithm
  - greedy
  - interval
categories:
  - algorithm
date: 2021-03-01 18:13:00
---
## 区间选点 贪心
[题目信息](https://www.acwing.com/problem/content/description/907/)  
给定N个闭区间[a<sub>i</sub>,b<sub>i</sub>]，请你在数轴上选择尽量少的点，使得每个区间内至少包含一个选出的点。

输出选择的点的最小数量。

位于区间端点上的点也算作区间内。

输入格式
第一行包含整数N，表示区间数。

接下来N行，每行包含两个整数a<sub>i</sub>,b<sub>i</sub>，表示一个区间的两个端点。

输出格式
输出一个整数，表示所需的点的最小数量。

输入样例：
```
3
-1 1
2 4
3 5
```
输出样例：
```
2
```
### 题意及思路
题意：给出一些线段，在这些线段上放置一个点来标志这个线段被覆盖。要求出如何用最少的点来覆盖所有的线段。
思路：将线段重叠最多的位置上放入点，这样所需要点的数量最少。可以试试贪心，因为这样满足局部最优解，而且也没有找到反例。
### 实现
那么怎样才能找到线段重叠的位置。我们需要对杂乱的线段进行排序，这样可以模拟出区间的覆盖过程。那么排序的关键字是起始位置还是终止位置。其实两种方案都行，这里我先以按终止位置升序来说明。
#### 为什么可以按照终止位置来升序：
首先我们先记录起始的一条线段的终点end，我们称为当前线段。
因为终止位置记录着当前线段所能往右延伸的最大距离。这时后来遍历到的线段只存在两种情况：
1. 线段起点小于或等于（可以等于是题目说明了端点处也算重叠）当前记录的线段终点，这条线段就必定会与当前线段存在交集。这得益于我们按照了终止位置升序排序，因为后来遍历到的线段终点位置大于等于当前线段。
2. 线段起点大于当前线段终点，这时两条线段不存在交集了。在当前线段的终止位置打点就是最优解，此后将遍历到的线段更新为当前线段。 
图解
![区间选点.jpg](https://i.loli.net/2021/03/01/m4aH2KDfZs3iwRj.jpg)

```C++
#include <iostream>
#include <algorithm>
#include <vector>

using namespace std;
#define input freopen("C:\\Users\\Administrator\\Desktop\\a.txt","r",stdin)

int main() {
    //input;
    vector<pair<int, int>> d; //记录线段
    int a, b, n;
    scanf("%d", &n);
    for (int i = 0; i < n; ++i) {
        scanf("%d%d", &a, &b);
        d.emplace_back(a, b);
    }
    sort(d.begin(), d.end(), [](const pair<int, int> &a, const pair<int, int> &b) { //线段排序
        return a.second < b.second;//按关键字终点位置递增
    });
    int end = d[0].second, ans = 0;
    for (int i = 0; i < d.size(); ++i) {
        if (d[i].first > end) { //遍历到的线段起点大于当前线段终点, 说明无重叠区域, 打点后更新线段
            ++ans;
            end = d[i].second;
        }
    }
    ++ans; //将最后一个线段打点
    printf("%d", ans);
    return 0;
}



```
#### 为什么也可以按照起点位置来排序：
经过上述的分析，我们的贪心策略是在当前线段的末尾放点是最优的。让我们来看看如过按照起点升序排列会遇到什么情况。
![区间选点2.jpg](https://i.loli.net/2021/03/01/FJZBUve9GtAk4fO.jpg)
但是贪心策略又是与当前线段的末尾端点有关。这时我们只需要动态更新一下当前线段与遍历到的线段的最小末尾端点即可。即```end = min(end, d[i].second);```

```C++
#include <iostream>
#include <algorithm>
#include <vector>

using namespace std;
#define input freopen("C:\\Users\\Administrator\\Desktop\\a.txt","r",stdin)

int main() {
    //input;
    vector<pair<int, int>> d; //记录线段
    int a, b, n;
    scanf("%d", &n);
    for (int i = 0; i < n; ++i) {
        scanf("%d%d", &a, &b);
        d.emplace_back(a, b);
    }
    sort(d.begin(), d.end(), [](const pair<int, int> &a, const pair<int, int> &b) { //线段排序
        if (a.first != b.first)
            return a.second < b.second;//主要关键字起点递增
    });
    int end = d[0].second, ans = 0; //end记录当前线段所能覆盖到的终止位置
    for (int i = 0; i < d.size(); ++i) {
        if (d[i].first > end) { //遍历到的线段起点大于等于当前线段终点, 说明无重叠区域, 打点后更新线段
            ++ans;
            end = d[i].second;
        }
        end = min(end, d[i].second);
    }
    ++ans; //将最后一个线段打点
    printf("%d", ans);
    return 0;
}


```