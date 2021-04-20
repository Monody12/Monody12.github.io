title: 第八届蓝桥杯大赛软件类省赛 C/C++ 大学B 组
author: Monody12
tags:
  - Blue Bridge Cup
  - prime
  - enumeration
  - math
  - knapsack
categories:
  - algorithm
date: 2021-04-08 12:17:00
---
## <a name="目录">目录</a>
1. [购物单](#购物单)
2. [等差素数列](#等差素数列)
3. [承压计算](#承压计算)
4. [方格分割](#方格分割)
5. [取位数](#取位数)
6. [最大公共子串](#最大公共子串)
7. [日期问题](#日期问题)
8. [包子凑数](#包子凑数)
9. [分巧克力](#分巧克力)
10. [k倍区间](#k倍区间)

## <a name="购物单">1.购物单</a>
使用记事本或者其他文本编辑软件的**替换功能**将xx折或者半价替换为价格倍率(如0.5)，然后编写简单的乘加程序即可。

```c++
#include <cstdio>

#define input freopen("C:\\Users\\Administrator\\Desktop\\a.txt","r",stdin)

using namespace std;

int main() {
    input;
    double sum = 0;
    for (int i = 0; i < 50; ++i) {
        double price, discount;
        scanf("%lf%lf", &price, &discount);
        sum += price * discount;
    }
    printf("%lf", sum);
    return 0;
}
```
输出答案：5136.859500
填入答案：5200


[回到顶部](#目录)

--------
## <a name="等差素数列">2.等差素数列 (线性筛 暴力)</a>
参见 [蓝桥杯 等差素数列](https://blog.csdn.net/weixin_40124642/article/details/79486442)
### 分析
使用线性筛求出100000以内的所有素数（因为也不知道答案范围有多大，姑且开这么大先）。然后枚举可能出现的公差，暴力求解。

```c++
#include <iostream>
#include <cstdio>

#define input freopen("C:\\Users\\Administrator\\Desktop\\a.txt","r",stdin)

using namespace std;
const int maxn = 1e5 + 1;
bool isprime[maxn];

void selectPrime(int n) {  //筛质数
    memset(isprime, true, sizeof(isprime));
    for (int i = 2,len = n / 2; i <= len; ++i)
        for (int j = 2 * i; j < n; j += i)
            isprime[j] = false;
}

int solve() {
    for (int i = 2; i < 1000; i++) {     //第一重循环公差
        for (int j = 2; j < maxn; j++) { //第二层循环设定 第一个素数
            int cnt = 0;   //计数器
            for (int k = 0; k < 10; k++) {  //如果满足  10 个素数输出
                if (isprime[j + i * k])
                    cnt++;
                if (cnt == 10) {
                    cout << i << " " << j << endl;
                    return i;
                }
            }
        }
    }
    return -1;
}

int main() {
    //input;
    selectPrime(maxn);
    int ans = solve();
    printf("%d", ans);
    return 0;
}

```

[回到顶部](#目录)

--------
## <a name="承压计算">3.承压计算 (模拟)</a>
X星球的高科技实验室中整齐地堆放着某批珍贵金属原料。

每块金属原料的外形、尺寸完全一致，但重量不同。
金属材料被严格地堆放成金字塔形。
```

                             7 
                            5 8 
                           7 8 8 
                          9 2 7 2 
                         8 1 4 9 1 
                        8 1 8 8 4 1 
                       7 9 6 1 4 5 4 
                      5 6 5 5 6 9 5 6 
                     5 5 4 7 9 3 5 5 1 
                    7 5 7 9 7 4 7 3 3 1 
                   4 6 4 5 5 8 8 3 2 4 3 
                  1 1 3 3 1 6 6 5 5 4 4 2 
                 9 9 9 2 1 9 1 9 2 9 5 7 9 
                4 3 3 7 7 9 3 6 1 3 8 8 3 7 
               3 6 8 1 5 3 9 5 8 3 8 1 8 3 3 
              8 3 2 3 3 5 5 8 5 4 2 8 6 7 6 9 
             8 1 8 1 8 4 6 2 2 1 7 9 4 2 3 3 4 
            2 8 4 2 2 9 9 2 8 3 4 9 6 3 9 4 6 9 
           7 9 7 4 9 7 6 6 2 8 9 4 1 8 1 7 2 1 6 
          9 2 8 6 4 2 7 9 5 4 1 2 5 1 7 3 9 8 3 3 
         5 2 1 6 7 9 3 2 8 9 5 5 6 6 6 2 1 8 7 9 9 
        6 7 1 8 8 7 5 3 6 5 4 7 3 4 6 7 8 1 3 2 7 4 
       2 2 6 3 5 3 4 9 2 4 5 7 6 6 3 2 7 2 4 8 5 5 4 
      7 4 4 5 8 3 3 8 1 8 6 3 2 1 6 2 6 4 6 3 8 2 9 6 
     1 2 4 1 3 3 5 3 4 9 6 3 8 6 5 9 1 5 3 2 6 8 8 5 3 
    2 2 7 9 3 3 2 8 6 9 8 4 4 9 5 8 2 6 3 4 8 4 9 3 8 8 
   7 7 7 9 7 5 2 7 9 2 5 1 9 2 6 5 3 9 3 5 7 3 5 4 2 8 9 
  7 7 6 6 8 7 5 5 8 2 4 7 7 4 7 2 6 9 2 1 8 2 9 8 5 7 3 6 
 5 9 4 5 5 7 5 5 6 3 5 3 9 5 8 9 5 4 1 2 6 1 4 3 5 3 2 4 1 
X X X X X X X X X X X X X X X X X X X X X X X X X X X X X X 
```

其中的数字代表金属块的重量（计量单位较大）。
最下一层的X代表30台极高精度的电子秤。

假设每块原料的重量都十分精确地平均落在下方的两个金属块上，
最后，所有的金属块的重量都严格精确地平分落在最底层的电子秤上。
电子秤的计量单位很小，所以显示的数字很大。

工作人员发现，其中读数最小的电子秤的示数为：2086458231

请你推算出：读数最大的电子秤的示数为多少？

注意：需要提交的是一个整数，不要填写任何多余的内容。

### 分析
题意：有30台高精度电子秤上成金字塔型放置着一些金属块，每一个金属块的种类都会平摊到下面的两个物品上。电子秤有一个自己的计量单位，给出了一个最小的电子秤读数，让我们求出一个最大的电子秤读数。

实现：使用数组来记录每一个物品和电子秤的重量（包含自身的重量和来自上方物品的重量）。先记录自身，再记录平摊。最后找出电子秤的最大重量和最小重量，计算出电子秤的显示倍率即可算出答案。

```C++
#include <algorithm>
#include <cstdio>
#include <climits>

#define input freopen("C:\\Users\\Administrator\\Desktop\\a.txt","r",stdin)

using namespace std;

double a[30][30];

int main() {
    input;
    for (int i = 0; i < 29; ++i) //读入每块重量
        for (int j = 0; j < i + 1; ++j)
            scanf("%lf", &a[i][j]);
    for (int i = 1; i < 30; ++i) { //均摊重量
        for (int j = 0; j < i + 1; ++j) {
            if (j == 0)
                a[i][j] += 0.5 * a[i - 1][j];
            else
                a[i][j] += 0.5 * (a[i - 1][j - 1] + a[i - 1][j]);
        }
    }
    double MaxWeight = DBL_MIN, MinWeight = DBL_MAX;
    for (int i = 0; i < 30; ++i) { //找出30台电子秤的最大承重和最小承重
        MaxWeight = max(MaxWeight, a[29][i]);
        MinWeight = min(MinWeight, a[29][i]);
    }
    double rate = 2086458231 / MinWeight; //电子秤的显示倍率
    printf("%lf", rate * MaxWeight);
    return 0;
}
```
输出答案：72665192664.000000  
填入答案：72665192664

[回到顶部](#目录)

--------
## <a name="方格分割">4.方格分割</a>


[回到顶部](#目录)

--------
## <a name="取位数">5.取位数</a>

求1个整数的第k位数字有很多种方法。  
以下的方法就是一种。  


// 求x用10进制表示时的数位长度   
```c
int len(int x){
	if(x<10) return 1;
	return len(x/10)+1;
}
	
// 取x的第k位数字
int f(int x, int k){
	if(len(x)-k==0) return x%10;
	return _____________________;  //填空
}
	
int main()
{
	int x = 23574;
	printf("%d\n", f(x,3));
	return 0;
}
```

对于题目中的测试数据，应该打印5。

请仔细分析源码，并补充划线部分所缺少的代码。

注意：只提交缺失的代码，不要填写任何已有内容或说明性的文字。

### 分析
我感觉这题有一点歧义，就是题中要求的第k位数是从左往右数k位数，还是从右往左数k位数。这里就姑且认为是从左往右吧。  
第一个函数是求一个整数的位数。看第二个函数的if条件，如果这个整数的长度等于k，就返回这个整数的最后一位。那么就可以推测出结果是：每次将这个数移除最后最后一位，直至这个数只有k位。

填入答案：`f(x/10,k)`

[回到顶部](#目录)

--------
## <a name="最大公共子串">6.最大公共子串</a>

最大公共子串长度问题就是：  
求两个串的所有子串中能够匹配上的最大长度是多少。  

比如："abcdkkk" 和 "baabcdadabc"，  
可以找到的最长的公共子串是"abcd",所以最大公共子串长度为4。  

下面的程序是采用矩阵法进行求解的，这对串的规模不大的情况还是比较有效的解法。  

请分析该解法的思路，并补全划线部分缺失的代码。 

```c
#include <stdio.h>
#include <string.h>

#define N 256
int f(const char* s1, const char* s2)
{
	int a[N][N];
	int len1 = strlen(s1);
	int len2 = strlen(s2);
	int i,j;
	
	memset(a,0,sizeof(int)*N*N);
	int max = 0;
	for(i=1; i<=len1; i++){
		for(j=1; j<=len2; j++){
			if(s1[i-1]==s2[j-1]) {
				a[i][j] = __________________________;  //填空
				if(a[i][j] > max) max = a[i][j];
			}
		}
	}
	
	return max;
}

int main()
{
	printf("%d\n", f("abcdkkk", "baabcdadabc"));
	return 0;
}
```

### 分析
参考学习[最长公共子串问题——矩阵法求解](https://blog.csdn.net/sinat_40872274/article/details/88093668#google_vignette)  
1、把两个字符串分别以行和列组成一个二维矩阵。

2、比较二维矩阵中每个点对应行列字符中否相等，相等的话值设置为1，否则设置为0。

3、通过查找出值为1的最长对角线就能找到最长公共子串。

所以如果发现字符串的两字符匹配，就根据在上一步数值（左上方）的基础上+1。

填入答案：`a[i-1][j-1] +1`


[回到顶部](#目录)

--------
## <a name="日期问题">7.日期问题 （模拟）</a>

小明正在整理一批历史文献。这些历史文献中出现了很多日期。小明知道这些日期都在1960年1月1日至2059年12月31日。令小明头疼的是，这些日期采用的格式非常不统一，有采用年/月/日的，有采用月/日/年的，还有采用日/月/年的。更加麻烦的是，年份也都省略了前两位，使得文献上的一个日期，存在很多可能的日期与其对应。  

比如02/03/04，可能是2002年03月04日、2004年02月03日或2004年03月02日。  

给出一个文献上的日期，你能帮助小明判断有哪些可能的日期对其对应吗？

输入

一个日期，格式是"AA/BB/CC"。  (0 <= A, B, C <= 9)  

输入

输出若干个不相同的日期，每个日期一行，格式是"yyyy-MM-dd"。多个日期按从早到晚排列。  

样例输入

02/03/04  

样例输出

2002-03-04  
2004-02-03  
2004-03-02  

### 分析
存在多种日期的排列方式，可以设计多种函数来分别处理不同排序。  
为了验证正确性使用了蓝桥练习系统的评测（不支持C++11特性）。

```C++
#include <iostream>
#include <vector>
#include <algorithm>
#include <string>
#include <cstdio>

#define input freopen("C:\\Users\\Administrator\\Desktop\\a.txt","r",stdin)

using namespace std;

string to_string(int n) { //整数转字符串函数
    string s;
    while (n) {
        int t = n % 10;
        n /= 10;
        s += (char) (t + '0');
    }
    reverse(s.begin(), s.end());
    return s;
}

int days[][13] = {{0, 31, 28, 31, 30, 31, 30, 31, 31, 30, 31, 30, 31},
                  {0, 31, 29, 31, 30, 31, 30, 31, 31, 30, 31, 30, 31}};

bool check_leapYear(int n) { //检查是否为闰年
    if (n % 4 == 0 && n % 100 != 0 || n % 400 == 0)
        return true;
    else return false;
}

string int_to_string(int n) { //整数转成日期字符串 (添加前导0)
    if (n < 10)
        return "0" + ::to_string(n);
    else return ::to_string(n);
}

string year_month_day(int year, int month, int day) {  //年/月/日
    if (year <= 60)
        year += 2000;
    else
        year += 1900;
    if (month > 12)
        return "";
    int leap = check_leapYear(year);
    if (days[leap][month] < day || day <= 0)
        return "";
    return ::to_string(year) + "-" + int_to_string(month) + "-" + int_to_string(day);
}

string month_day_year(int month, int day, int year) {  //月/日/年
    if (year <= 60)
        year += 2000;
    else
        year += 1900;
    if (month > 12)
        return "";
    int leap = check_leapYear(year);
    if (days[leap][month] < day || day <= 0)
        return "";
    return ::to_string(year) + "-" + int_to_string(month) + "-" + int_to_string(day);
}

string day_month_year(int day, int month, int year) {  //日/月/年
    if (year <= 60)
        year += 2000;
    else
        year += 1900;
    if (month > 12)
        return "";
    int leap = check_leapYear(year);
    if (days[leap][month] < day || day <= 0)
        return "";
    return ::to_string(year) + "-" + int_to_string(month) + "-" + int_to_string(day);
}

int main() {
    //input;
    int one, two, three;
    scanf("%d/%d/%d", &one, &two, &three);
    vector<string> ans;
    string tmp = year_month_day(one, two, three);
    if (tmp.length() != 0)
        ans.push_back(tmp);
    tmp = day_month_year(one, two, three);
    if (tmp.length() != 0 && one != three)
        ans.push_back(tmp);
    tmp = month_day_year(one, two, three);
    if (tmp.length() != 0 && one != two && two != three)
        ans.push_back(tmp);
    sort(ans.begin(), ans.end());
    for (int i = 0; i < ans.size(); ++i)
        cout << ans[i] << endl;
    return 0;
}
```


[回到顶部](#目录)

--------
## <a name="包子凑数">8.包子凑数 (数学 完全背包)</a>

小明几乎每天早晨都会在一家包子铺吃早餐。他发现这家包子铺有N种蒸笼，其中第i种蒸笼恰好能放Ai个包子。每种蒸笼都有非常多笼，可以认为是无限笼。

每当有顾客想买X个包子，卖包子的大叔就会迅速选出若干笼包子来，使得这若干笼中恰好一共有X个包子。比如一共有3种蒸笼，分别能放3、4和5个包子。当顾客想买11个包子时，大叔就会选2笼3个的再加1笼5个的（也可能选出1笼3个的再加2笼4个的）。

当然有时包子大叔无论如何也凑不出顾客想买的数量。比如一共有3种蒸笼，分别能放4、5和6个包子。而顾客想买7个包子时，大叔就凑不出来了。

小明想知道一共有多少种数目是包子大叔凑不出来的。

输入

第一行包含一个整数N。(1 <= N <= 100)
以下N行每行包含一个整数Ai。(1 <= Ai <= 100)  

输出

一个整数代表答案。如果凑不出的数目有无限多个，输出INF。

例如，  
输入：  
2  
4  
5   

程序应该输出：
6  

再例如，  
输入：  
2  
4  
6    

程序应该输出：
INF

样例解释：
对于样例1，凑不出的数目包括：1, 2, 3, 6, 7, 11。  
对于样例2，所有奇数都凑不出来，所以有无限多个。  

### 分析
题意：这是一个凑数问题，要求利用现有的数能否凑成任意一个数，每个现有的数有无限多个。   
显然是一个完全背包问题，但问题是背包的dp数组上限要开多大。这里涉及到一个数学知识，如果所给的数它们的最大公因数GCD不为1，说明它们都是一个不为1的数的倍数。那么显然存在无限多个正整数不是GCD的倍数，所以这种情况时输出INF。若GCD为1，由Ai最大为100可得，dp开100<sup>2</sup>就行。

```c++
#include <iostream>
#include <vector>
#include <algorithm>
#include <string>
#include <cstdio>

#define input freopen("C:\\Users\\Administrator\\Desktop\\a.txt","r",stdin)

using namespace std;

int a[101];
bool dp[10001] = {true}; //dp数组含义 dp[i]代表客人需要i个包子时能否用现有蒸笼凑出来

int gcd(int a, int b) { //求最大公约数
    return b ? gcd(b, a % b) : a;
}

int main() {
    //input;
    int n;
    scanf("%d", &n);
    for (int i = 0; i < n; ++i)
        scanf("%d", a + i);
    int GCD = a[0];
    for (int i = 0; i < n; ++i) {
        GCD = gcd(GCD, a[i]);  //求所有蒸笼能存放包子的数量的最大公因数
    }
    if (GCD != 1) { //如果最大公因数不为1，就会有无穷组不存在的情况
        printf("INF");
        exit(0);
    }
    for (int i = 0; i < n; ++i) // 完全背包
        for (int j = a[i]; j < 10001; ++j)
            if (dp[j - a[i]] == true)
                dp[j] = true;
    int ans = 0;
    for (int i = 0; i < 10001; ++i)
        if (dp[i] == false)
            ++ans;
    printf("%d", ans);
    return 0;
}
```


[回到顶部](#目录)

--------
## <a name="分巧克力">9.分巧克力</a>
儿童节那天有K位小朋友到小明家做客。小明拿出了珍藏的巧克力招待小朋友们。
小明一共有N块巧克力，其中第i块是Hi x Wi的方格组成的长方形。

为了公平起见，小明需要从这 N 块巧克力中切出K块巧克力分给小朋友们。切出的巧克力需要满足：

    1. 形状是正方形，边长是整数  
    2. 大小相同  

例如一块6x5的巧克力可以切出6块2x2的巧克力或者2块3x3的巧克力。

当然小朋友们都希望得到的巧克力尽可能大，你能帮小Hi计算出最大的边长是多少么？

输入
第一行包含两个整数N和K。(1 <= N, K <= 100000)  
以下N行每行包含两个整数Hi和Wi。(1 <= Hi, Wi <= 100000) 
输入保证每位小朋友至少能获得一块1x1的巧克力。   

输出
输出切出的正方形巧克力最大可能的边长。

样例输入：
2 10  
6 5  
5 6  

样例输出：
2

```c++
#include <cstdio>
#include <vector>
#include <cmath>

#define input freopen("C:\\Users\\Administrator\\Desktop\\a.txt","r",stdin)

using namespace std;

const int maxn = 100005;

/**
 * 判断以边长a来分巧克力是否能满足k个小朋友的需求
 * @param chocolates 巧克力的信息
 * @param n 有n块巧克力
 * @param k k个小朋友
 * @param a 分的边长
 * @return 能否满足要求
 */
bool check(vector<pair<int, int>> &chocolates, int n, int k, int a) {
    int sum = 0;
    for (pair<int, int> &i:chocolates) {
        int height = i.first / a; //在高度上能切多少刀
        int width = i.second / a; //宽度上能切多少刀
        sum += height * width;
        if (sum >= k)
            return true;
    }
    return false;
}

int main() {
    //input;
    int n, k;
    scanf("%d%d", &n, &k);
    vector<pair<int, int>> chocolates(n);
    for (int i = 0; i < n; ++i) {
        scanf("%d%d", &chocolates[i].first, &chocolates[i].second);
    }
    int l = 1, r = maxn, mid;
    while (l <= r) {
        mid = (l + r) / 2;
        if (check(chocolates, n, k, mid))
            l = mid + 1;
        else
            r = mid - 1;
    }
    printf("%d", r);
    return 0;
}

```
[回到顶部](#目录)

--------
## <a name="k倍区间">10.k倍区间</a>


[回到顶部](#目录)

--------