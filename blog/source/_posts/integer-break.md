title: 整数拆分 (贪心 动态规划)
author: Monody12
tags:
  - algorithm
  - greedy
  - ' dynamic programming'
categories:
  - algorithm
date: 2021-03-11 08:51:00
---
# 整数拆分求最大乘积

## 拆分为可以相同的正整数
### 343. 整数拆分
[题目信息](https://leetcode-cn.com/problems/integer-break/)
给定一个正整数 n，将其拆分为至少两个正整数的和，并使这些整数的乘积最大化。 返回你可以获得的最大乘积。

示例 1:
```
输入: 2
输出: 1
解释: 2 = 1 + 1, 1 × 1 = 1。
```
示例 2:
```
输入: 10
输出: 36
解释: 10 = 3 + 3 + 4, 3 × 3 × 4 = 36。
```
说明: 你可以假设 n 不小于 2 且不大于 58。

#### 解法
##### 1. 贪心
+ 数字1~4的时候可以不用拆分。保持原数乘积即为最大值
+ 数字5可以拆分为 2*3
+ 数字6可以拆分为 3*3
+ 数字7可以拆分为 3*4
+ 数字8可以拆分为 3*3*2  
......  
归纳证明（[参考](https://leetcode-cn.com/problems/integer-break/solution/zheng-shu-chai-fen-by-leetcode-solution/)）：显然，尽可能的拆分出多个3满足局部最优。而由于n是可以拆分为1,2,3,4,5,6.....这些数字的，所以局部最优可以推导出全局最优。

贪心策略：将5或以上的数，拆分出尽可能多的3，直至n被拆分得只剩下4以下。

```C++
class Solution {
public:
    int integerBreak(int n) {
        if (n <= 3) {
            return n - 1;
        }
        int quotient = n / 3;
        int remainder = n % 3;
        if (remainder == 0) {
            return (int)pow(3, quotient);
        } else if (remainder == 1) {
            return (int)pow(3, quotient - 1) * 4;
        } else {
            return (int)pow(3, quotient) * 2;
        }
    }
};

```

##### 2. 动态规划
由于数据范围很小： 2 ≤ n ≤ 58。
我们可以使用动态规划，不断尝试如何拆分为最优。
###### dp数组含义

记dp[i]为i拆分出的乘积为最优解。

将i不断的拆分出j。

每一个数的乘积最大值可以考虑由拆分得来，或者不拆分得来。

###### 递推公式  
不拆分：(i - j) \* j  
拆分 ： dp[i - j] \* j (dp数组表示拆分后的最大乘积)

所以递推公式为 ```dp[i] = max(dp[i], max(dp[i - j] * j, (i - j) * j));```

###### 初始化  
由于至少拆分出两个正整数，所以0和1无意义。初始化 dp[2]=1

###### 遍历顺序
由于大数可以拆分成小数。小数的最优解即使本题动态规划需要利用子问题。所以遍历从小到大。

```C++
class Solution {
public:
    int integerBreak(int n) {
        vector<int> dp(n + 1);
        dp[2] = 1;
        for (int i = 3; i <= n; ++i) { //dp遍历过程,从前往后, 因为后面会用到之前的结论, 例如dp[i-j]
            for (int j = 1; j < i; ++j) { //拆分过程
                dp[i] = max(dp[i], max(dp[i - j] * j, (i - j) * j));
            }
        }
        return dp[n];
    }
};
```

## 拆分为互不相同的自然数
[题目描述](https://www.luogu.com.cn/problem/P1249)
一个正整数一般可以分为几个互不相同的自然数的和，如3 = 1 + 2，4 = 1 + 3，5＝ 1 + 4 = 2 + 3，6 =1 + 5＝2 + 4。

现在你的任务是将指定的正整数 n 分解成若干个互不相同的自然数的和，且使这些自然数的乘积最大。

输入格式
只一个正整数 n，（3 ≤ n ≤ 10000）。

输出格式
第一行是分解方案，相邻的数之间用一个空格分开，并且按由小到大的顺序。

第二行是最大的乘积。

示例1：  
输入：10  
输出：   
2 3 5  
30

#### 解法
有没有看起来和上一题很相似。但是数据范围非常大。假设输入的数是5050，那么可以拆分成1，2，3，...,100，乘积为100!（的阶乘）。不仅运算结果会非常大，而且如果使用动态规划的枚举拆分会有O(n<sup>2</sup>)的复杂度，还会导致超时。因此算法可以直接考虑贪心！

就用拿样例来分析吧。给定n=10，要怎么拆分才能获得最大乘积。因为1乘任何数都等于任何数，所以不需要拆除一个1 。直接从2开始拆，假设我们随便拆分，得到2，8，乘积为16 。我们再继续拆，发现8还可以拆出3,4。（因为不能重复嘛，所以不能继续拆出2了），这时候我乘积为24 ，还余一个1。那么这个1是分配到哪个数上好呢。好像还不能任选，只能从后往前分配。因为分配到前面的数会导致和后来的数发生冲突。所以最终的结果为2，3，5 。答案为30 。

上面只是基于初步的分析，至于为什么要把大数尽量拆成小数，是有数学公式及严格的证明的。公式为```n(n+1) > (n-1)(n+2)```，数学不太好在这里就不去证明了。有了这个公式就可以解释为什么要尽量拆成从小到大的数了。

思路就介绍到这里，因为C/C++的长整型不满足于计算出比100的阶乘还要大的数，所以我选择了自带高精度的Java。

```Java
import java.io.BufferedInputStream;
import java.math.BigInteger;
import java.util.ArrayList;
import java.util.Scanner;

public class Main {

    public static void main(String[] args) {
        Scanner input = new Scanner(new BufferedInputStream(System.in));
        int n = input.nextInt();
        ArrayList<Integer> list = new ArrayList<>(); //存储拆分出来的数
        int last = n; //n可拆分的大小
        for (int i = 2; i <= last; ++i) { //贪心: 从小到大拆分出不同的自然数
            list.add(i);
            last -= i;
        }
        //贪心 n(n+1) > (n-1)(n+2)
        for (int i = list.size() - 1; last > 0 && i >= 0; --i) { //将未分配的值从列表的后往前一次分配1
            --last;
            list.set(i, list.get(i) + 1);
        }
        BigInteger product = new BigInteger("1"); //乘积
        for (Integer integer : list) {
            BigInteger t = new BigInteger(Integer.toString(integer));
            System.out.print(t + " ");
            product = product.multiply(t);
        }
        System.out.print("\n" + product);
    }
}
```


 

