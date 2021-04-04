title: 二分查找 （模板）
author: Monody12
tags:
  - algorithm
  - Binary search
categories:
  - algorithm
date: 2021-04-01 20:23:00
---
### 应用：
#### 1.二分查找技术
#### 2.判断可行性

### 一、二分查找技术
应用：在有序数列中查找一个数是否出现过，或者查找一个数在这个有序数列中应该排在的位置。

原理：就拿猜数游戏来举例吧。在1-100这个100个数字中猜对面手上写的是什么数，你每说出一个数，对方就会告诉你你猜大了或者是猜小了。如果你漫无目的的猜，或者是从1开始往后猜，每次加几个数，很有可能猜了二三十次也没有猜对。而如果你充分利用猜大了或者是猜小了这个反馈机制。  
假设第一次猜50，如果小了就猜75。大了就猜(50+75)/2=62 。这样每次都可以最大化的排除一半的错误区间。在log<sub>2</sub>100次以内就必定能猜中。**每次排除一半的错误区间**——这就是二分！

示例代码
```C++
#include <cstdio>
#include <algorithm>
#include <vector>

using namespace std;

int main() {
    vector<int> a{1, 5, 6, 2, 3, 6, 7, 98, 8, 4, 10, 132, 584, 123, 45, 77};
    sort(a.begin(), a.end());
    int l = 0, r = a.size() - 1, mid;
    int x;
    puts("输入要查找的数值");
    scanf("%d", &x);
    while (l <= r) {
        mid = (l + r) / 2;
        if (a[mid] > x)
            r = mid - 1;
        else if (a[mid] < x)
            l = mid + 1;
        else  //找到了这个数
            break;
    }
    puts("升序序列: ");
    for (int i = 0; i < a.size(); ++i)
        printf("%d: %d\n", i, a[i]);
    if (a[l] != x)
        puts("找不到该数");
    puts("输出这个数在升序序列中应该排的位置");
    printf("%d ", l);
    return 0;
}

```

----------
### 二、二分可行性
暂无内容