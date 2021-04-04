title: 跳跃游戏 II （贪心）
author: Monody12
tags:
  - algorithm
  - greedy
categories:
  - algorithm
date: 2021-02-21 13:04:00
---
## 力扣45.跳跃游戏 II
##### [题目信息](https://leetcode-cn.com/problems/jump-game-ii/)
给定一个非负整数数组，你最初位于数组的第一个位置。

数组中的每个元素代表你在该位置可以跳跃的最大长度。

你的目标是使用最少的跳跃次数到达数组的最后一个位置。

示例:
```
输入: [2,3,1,1,4]
输出: 2
解释: 跳到最后一个位置的最小跳跃数是 2。
     从下标为 0 跳到下标为 1 的位置，跳 1 步，然后跳 3 步到达数组的最后一个位置。
```
说明:  
假设你总是可以到达数组的最后一个位置。

---

### 解法
分析：本题为[跳跃游戏](../../../../2021/02/20/跳跃游戏题解/)的进阶版，我们不需要考虑能否从数组起始位置到达数组末尾，但是要求出最小跳跃次数。在我们到终点的路径上，如果我们每次都**以最小的步数走到能走到的地方**（贪心：**局部最优**），那么我们到达终点时必定就是最小的步数（全局最优）。那么我们要怎么做才能保证每一次走出去都是以最小的步数来走的。  

实现：从头开始向后走。每走到一个点，都会向后覆盖一段距离```[i+1,i+nums[i]]```，那么这一段距离的最小步数，就为当前点的最小步数+1。我们可以使用数组step来维护每个点的最小步数。如图：  
![跳跃游戏II_1.jpg](https://i.loli.net/2021/02/21/dFmgIc2P1xZDV9S.jpg)

代码 （带注释版，将25行注释消除即可显示维护过程）
```C++
class Solution {
private:
    void print(int n, int *nums) {
        for (int i = 0; i <= n; ++i) {
            printf("%d ", nums[i]);
        }
        putchar(10);
    }

public:
    int jump(vector<int> &nums) {
        int distance = 0, len = nums.size() - 1; //分别为 当前可以前进的距离, 总共需要跳过的元素数量
        int *step = new int[nums.size()];// 维护最小步数
        memset(step + 1, -1, sizeof(int) * len); //初始化
        step[0] = 0;// 起始位置不需要跳
        for (int i = 0; i < len; ++i) {
            int d = i + nums[i];//站在当前点上能到达的最大距离
            if (d >= len) {
                step[len] = step[i] + 1; //在当前点再跳一步即可到终点
                break;
            }
            if (d > distance) {  //当前遍历的距离大于目前能跳的最大距离
                fill(step + distance + 1, step + d + 1, step[i] + 1);//达到这个最大距离需要多花费一步
                distance = d; //更新这个最大距离
                //print(min(distance, len), step);
            }
        }
        int ans = step[len];
        delete [] step;
        return ans;
    }
};

```

简洁版：
```C++
class Solution {
public:
    int jump(vector<int> &nums) {
        int distance = 0, len = nums.size() - 1;
        int *step = new int[nums.size()];
        memset(step + 1, -1, sizeof(int) * len);
        step[0] = 0;
        for (int i = 0; i < len; ++i) {
            int d = i + nums[i];
            if (d >= len) {
                step[len] = step[i] + 1;
                break;
            }
            if (d > distance) {
                fill(step + distance + 1, step + d + 1, step[i] + 1);
                distance = d;
            }
        }
        int ans = step[len];
        delete [] step;
        return ans;
    }
};
```
### 内存使用优化

分析：在上面的代码中，我们使用了数组维护到达每一个点的最小跳跃次数。我们发现：在一段区间内，最小跳跃次数都是一样的，我们记录下来这个区间的起点和终点不就可以省区这开数组的O(n)空间了。

实现：使用distance来记录当前步数最大能到达的位置，nextStepDistance来记录下一步最大能到达的位置。当遍历到distance时，考虑是否已走到终点，若没走到，则还需要再走下一步（此时更新distance即可）。

图示：小括号为当前步数能到达的位置，中括号为下一步能到达的位置。小/中括号的最后一个元素对应于distance、nextStepDistance。
![跳跃游戏II_2.jpg](https://i.loli.net/2021/02/21/vdYjRHBgWcSVKeT.jpg)

代码（含注释）：
```C++
class Solution {
public:
    int jump(vector<int> &nums) {
        int distance = 0, len = nums.size() - 1; //分别为当前可以前进的距离, 总共需要跳过的元素数量
        int step = 0, nextStepDistance = 0;//step记录当前步数, nextStepDistance记录下一步的终点
        for (int i = 0; i < len; ++i) {
            int d = i + nums[i];//站在当前点上能到达的最大距离
            if (d > nextStepDistance) //当前遍历的距离大于目前能跳的最大距离
                nextStepDistance = d; //更新这个最大距离
            if (i == distance) {//到达当前步数能走的最大位置
                if (distance < len) { //当前还未走到终点, 继续走下一步
                    ++step;
                    distance = nextStepDistance;
                } else
                    break;
            }
        }
        return step;
    }
};
```

代码（不含注释）：
```C++
class Solution {
public:
    int jump(vector<int> &nums) {
        int distance = 0, len = nums.size() - 1; 
        int step = 0, nextStepDistance = 0;
        for (int i = 0; i < len; ++i) {
            int d = i + nums[i];
            if (d > nextStepDistance) 
                nextStepDistance = d; 
            if (i == distance) {
                if (distance < len) {
                    ++step;
                    distance = nextStepDistance;
                } else
                    break;
            }
        }
        return step;
    }
};
```