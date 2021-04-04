title: 跳跃游戏 （模拟 贪心）
author: Monody12
tags:
  - algorithm
  - greedy
  - simulation
categories:
  - algorithm
date: 2021-02-20 20:30:00
---
## 力扣55.跳跃游戏
##### [题目信息](https://leetcode-cn.com/problems/jump-game/)
给定一个非负整数数组 nums ，你最初位于数组的 第一个下标 。

数组中的每个元素代表你在该位置可以跳跃的最大长度。

判断你是否能够到达最后一个下标。

 

示例 1：
```
输入：nums = [2,3,1,1,4]  
输出：true  
解释：可以先跳 1 步，从下标 0 到达下标 1, 然后再从下标 1 跳 3 步到达最后一个下标。  
```
示例 2：
```
输入：nums = [3,2,1,0,4]
输出：false
解释：无论怎样，总会到达下标为 3 的位置。但该下标的最大跳跃长度是 0 ， 所以永远不可能到达最后一个下标。
```
---
### 解法
 #### 1.  模拟
分析：若能从起点到达终点，那么必定存在至少一条能从起点到终点的路径。要求出这一条具体路径吗？其实不用的，题目只要我们判断能否到达终点就行。这时候我们可以将问题转化为，能否从终点前面的任意一点到达终点即可。  

实现：从终点开始向倒退，寻找能否存在一点能够到达终点，若找到，则可以将这一点更新为新的终点lastIndex。若新的终点最终被更新为0，则说明存在从起点到终点的路径。
![跳跃游戏图解.png](https://i.loli.net/2021/02/20/V5PzTM6NClF4otv.png)  
```c++
class Solution {
public:
    bool canJump(vector<int> &nums) {
        int lastIndex = nums.size() - 1;
        for (int i = lastIndex - 1; i >= 0; --i) {
            if (i + nums[i] >= lastIndex)
                lastIndex = i;
        }
        if (lastIndex == 0) //能从终点退回起点
            return true;
        else return false;
    }
};
```
#### 2. 贪心
分析：根据题意，站在一点所能向前跳跃的距离取决于该点的值。即一个点能向前覆盖```nums[i]```个范围，那么我们可以基于贪心思想，每次想着跳出最远距离（贪心思想：**局部最优**），动态求出能向前覆盖的最大范围（**全局最优**）。当覆盖的范围大于所有点的数量时，就认为能跳到终点。
实现：使用一个变量```maxDistance```来描述从0位置向前跳跃的距离，然后动态更新这个值。  
注意：需要特判一下只有一个元素的情况。
```C++
class Solution {
public:
    bool canJump(vector<int> &nums) {
        if (nums.size() == 1)  //只有一个元素时, 第一个点就是最后一个点, 满足要求
            return true;
        int maxDistance = 0;
        for (int i = 0; i <= maxDistance; ++i) { //所能经过的点下标上限为当前的最大跳跃距离
            maxDistance = max(maxDistance, i + nums[i]); //更新最大跳跃距离
            if (maxDistance >= nums.size() - 1)
                return true;
        }
        return false;
    }
};
```