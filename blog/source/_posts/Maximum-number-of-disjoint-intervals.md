title: 最大不相交区间数量 (贪心)
author: Monody12
tags:
  - interval
  - algorithm
  - greedy
categories:
  - algorithm
date: 2021-03-01 20:01:00
---
## 最大不相交区间数量 贪心
[题目信息](https://www.acwing.com/problem/content/910/)  
给定N个闭区间[a<sub>i</sub>,b<sub>i</sub>]，请你在数轴上选择若干区间，使得选中的区间之间互不相交（包括端点）。

输出可选取区间的最大数量。

输入格式
第一行包含整数N，表示区间数。

接下来N行，每行包含两个整数a<sub>i</sub>,b<sub>i</sub>，表示一个区间的两个端点。

输出格式
输出一个整数，表示可选取区间的最大数量。

数据范围  
1≤N≤10<sup>5</sup>,
−109≤a<sub>i</sub>≤b<sub>i</sub>≤10<sup>9</sup>  
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
题意：在给出的一些线段区间中，选出尽可能多的线段使他们互不相交。
思路：要选出一些不重叠的线段。在一些相交的线段中，选择与其他线段相交冲突最小的即是局部最优解。这就可以考虑使用贪心算法了。
### 实现
那我们这样找到相交冲突最小的线段的。贪心涉及到排序，排序既可以按照线段的起点也可以按照终点，只不过遍历的方式不一样而已。
按照终点排序：遍历需要从左往右。因为终点边界数值越小，线段占用的空间越小，与后面的线段冲突的概率就越小。
按照起点排序：遍历需要从右至左。因为起点边界数值越大，线段占用的空间越小，与前面的线段冲突的概率就越小。

这里示例 按终点升序排序 从左至右遍历

遍历：遍历到的线段与当前线段的终点只有两种情况
1. 遍历到的线段起点小于等于（可以等于是题目说明了端点处也算相交）当前线段的终点，与当前线段**相交**。由于按照了终点升序排序，选择这一条线段不会比当前线段的解要更优，所以忽略掉就行。
2. 遍历到的线段起点大于当前的终点。与当前线段**不相交**。不相交区间计数器+1，将这条线段更新为当前线段。

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
        return a.second < b.second;//按终点递增
    });
    int end = d[0].second, ans = 0; //end记录当前线段所能覆盖到的终止位置
    for (int i = 0; i < d.size(); ++i) {
        if (d[i].first > end) { //遍历到的线段起点大于等于当前线段终点, 说明无重叠区域，选中这一条线段
            ++ans;
            end=d[i].second;
        }
    }
    ++ans; //将最后一个线段计入
    printf("%d", ans);
    return 0;
}

```