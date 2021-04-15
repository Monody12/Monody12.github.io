title: Untitled
author: Monody12
date: 2021-04-15 21:46:45
tags:
---
<a name="目录">目录</a>
1. [门牌制作](#门牌制作)
1. [既约分数](#既约分数)
1. [蛇形填数](#蛇形填数)
1. [跑步锻炼](#跑步锻炼)
1. [七段码](#七段码)
1. [成绩统计](#成绩统计)
1. [回文日期](#回文日期)
1. [子串分值和](#子串分值和)
1. [平面切分](#平面切分)
1. [字串排序](#字串排序)

## <a name="门牌制作">A: 门牌制作</a>

小蓝要为一条街的住户制作门牌号。  
这条街一共有2020 位住户，门牌号从1 到2020 编号。  
小蓝制作门牌的方法是先制作0 到9 这几个数字字符，最后根据需要将字
符粘贴到门牌上，例如门牌1017 需要依次粘贴字符1、0、1、7，即需要1 个
字符0，2 个字符1，1 个字符7。  
请问要制作所有的1 到2020 号门牌，总共需要多少个字符2  

### 分析
分离出每个1-2020这些整数中的2即可。
```c++
#include <algorithm>
#include <cstdio>

using namespace std;
#define input freopen("C:\\Users\\Administrator\\Desktop\\a.txt","r",stdin)

int getCount(int n) {
    int sum = 0;
    while (n) {
        sum += n % 10 == 2;
        n /= 10;
    }
    return sum;
}

int main() {
    //input;
    int sum = 0;
    for (int i = 1; i <= 2020; ++i) {
        sum += getCount(i);
    }
    printf("%d", sum);  //输出答案 624
    return 0;
}

```
输出答案：624

[回到顶部](#目录)

---------------
## <a name="既约分数">B: 既约分数 （GCD 最大公约数 枚举）</a>

![既约分数题目.jpg](https://i.loli.net/2021/04/15/QOqE5ySDeZv9HB3.jpg)

### 分析
本题难点是写出求最大公约数的函数，至于分子和分母嘛，全都枚举一遍就行。
```c++
#include <algorithm>
#include <cstdio>

using namespace std;
#define input freopen("C:\\Users\\Administrator\\Desktop\\a.txt","r",stdin)

int gcd(int a,int b){
    return b?gcd(b,a%b):a;
}

int main() {
    //input;
    int ans = 0;
    for (int i = 1; i <= 2020; ++i)  //枚举分子和分母
        for (int j = 1; j <= 2020; ++j)
            if (gcd(i,j)==1)
                ++ans;
    printf("%d",ans);  //输出2481215
    return 0;
}
```
输出答案：2481215

[回到顶部](#目录)

---------------
## <a name="蛇形填数">C: 蛇形填数</a>

![蛇形填数题目.jpg](https://i.loli.net/2021/04/15/IlOXLbn9Ysj1VWG.jpg)

[回到顶部](#目录)

---------------
## <a name="跑步锻炼">D: 跑步锻炼</a>

小蓝每天都锻炼身体。  
正常情况下，小蓝每天跑1 千米。如果某天是周一或者月初（1 日），为了
激励自己，小蓝要跑2 千米。如果同时是周一或月初，小蓝也是跑2 千米。
小蓝跑步已经坚持了很长时间，从2000 年1 月1 日周六（含）到2020 年
10 月1 日周四（含）。请问这段时间小蓝总共跑步多少千米？

### 分析
从2000年1月1日开始，一直模拟进行每一天跑步。判断是否是周一或者是月初的第一天，如果是就多跑一公里。  
实现：用一个day来记录今天是星期几，days数组记录闰年和非闰年每个月对应的天数。
注意细节：闰年判断

```c++
#include <cstdio>

using namespace std;
#define input freopen("C:\\Users\\Administrator\\Desktop\\a.txt","r",stdin)

int days[][13] = {{0, 31, 28, 31, 30, 31, 30, 31, 31, 30, 31, 30, 31},
                  {0, 31, 29, 31, 30, 31, 30, 31, 31, 30, 31, 30, 31}};  //闰年和非润内每个月的天数

bool is_leap(int year) {
    return year % 4 == 0 && year % 100 != 0 || year % 400 == 0;
}

int main() {
    //input;
    int day = 6, ans = 0;//day:今天是星期几
    for (int i = 2000; i <= 2019; ++i) {  //计算2000-2019年总共跑了多少千米
        int leap = is_leap(i);
        for (int j = 1; j <= 12; ++j) {  //枚举月份
            for (int k = 1; k <= days[leap][j]; ++k) {  //枚举天数
                if (k != 1 && day != 1)
                    ++ans;
                else ans += 2;
                day = (day + 1) % 7; //推进一天
            }

        }
    }
    int leap = is_leap(2020);
    for (int i = 1; i <= 9; ++i) {
        for (int j = 1; j <= days[leap][i]; ++j) {  //枚举天数
            if (j != 1 && day != 1)
                ++ans;
            else ans += 2;
            day = (day + 1) % 7; //推进一天
        }
    }
    ans += 2; //2020年10月1日当天
    printf("%d", ans);  //输出答案：8879
    return 0;
}
```

[回到顶部](#目录)

---------------
## <a name="七段码">E: 七段码</a>

[回到顶部](#目录)

---------------
## <a name="成绩统计">F: 成绩统计</a>

小蓝给学生们组织了一场考试，卷面总分为100 分，每个学生的得分都是
一个0 到100 的整数。
如果得分至少是60 分，则称为及格。如果得分至少为85 分，则称为优秀。
请计算及格率和优秀率，用百分数表示，百分号前的部分四舍五入保留整
数。

### 分析
感觉没什么好分析的，可以算作c语言入门题。  
注意：printf输出%号需要打两个%（即%%）。

```c++
#include <cstdio>

using namespace std;
#define input freopen("C:\\Users\\Administrator\\Desktop\\a.txt","r",stdin)


int main() {
    //input;
    int excellent = 0, good = 0, n, x;
    scanf("%d", &n);
    for (int i = 0; i < n; ++i) {
        scanf("%d", &x);
        if (x >= 85)
            ++excellent;
        if (x >= 60)
            ++good;
    }
    printf("%.0lf%%\n%.0lf%%", 1.0 * good / n * 100, 1.0 * excellent / n * 100);
    return 0;
}
```


[回到顶部](#目录)

---------------
## <a name="回文日期">G: 回文日期</a>

2020 年春节期间，有一个特殊的日期引起了大家的注意：2020 年2 月2
日。因为如果将这个日期按“yyyymmdd” 的格式写成一个8位数是20200202，
恰好是一个回文数。我们称这样的日期是回文日期。  
有人表示20200202 是“千年一遇” 的特殊日子。对此小明很不认同，因为
不到2 年之后就是下一个回文日期：20211202 即2021 年12 月2 日。
也有人表示20200202 并不仅仅是一个回文日期，还是一个ABABBABA
型的回文日期。对此小明也不认同，因为大约100年后就能遇到下一个
ABABBABA 型的回文日期：21211212 即2121 年12 月12 日。算不上“千
年一遇”，顶多算“千年两遇”。  
给定一个8 位数的日期，请你计算该日期之后下一个回文日期和下一个
ABABBABA 型的回文日期各是哪一天。

### 分析
我们需要寻找一个回文日期，不能从起始时间开始依次+1来判断，这样必定会超时。所以需要根据年份的反字符串来生成月份和天数。与前面的[跑步锻炼](#跑步锻炼)相似，需要判断闰年和获取每个月份的天数，这里需要检测生成的月份和天数是否合法。  
实现：开数组来存储闰年和非闰年对应的每个月份的天数，写一个判断闰年的函数。和一个字符串转整数的函数，目的是将日期的反字符串转成整数方便进行判断。检测ABAB型可以通过字符串来检测。

```c++
#include <cstdio>
#include <string>
#include <iostream>
#include <algorithm>

using namespace std;
#define input freopen("C:\\Users\\Administrator\\Desktop\\a.txt","r",stdin)

int days[][13] = {{0, 31, 28, 31, 30, 31, 30, 31, 31, 30, 31, 30, 31},
                  {0, 31, 29, 31, 30, 31, 30, 31, 31, 30, 31, 30, 31}};  //闰年和非润内每个月的天数

bool is_leap(int year) {  //判断闰年
    return year % 4 == 0 && year % 100 != 0 || year % 400 == 0;
}

int to_int(const string &s) {  //字符串转整数
    int sum = 0;
    for (char i : s) {
        sum *= 10;
        sum += i - '0';
    }
    return sum;
}

bool checkHuiWen(int year, const string &s_mmdd) {  //检查回文时日期是否正确
    int mmdd = to_int(s_mmdd);
    int month = mmdd / 100;
    int day = mmdd % 100;
    int leap = is_leap(year);
    if (month == 0 || month > 12)
        return false;
    if (day == 0 || day > days[leap][month])
        return false;
    return true;
}

bool check_ABAB(int year) {  //检查是否符合ABAB格式
    string s = to_string(year);
    return s[0] == s[2] && s[1] == s[3] && s[0] != s[1];
}

int main() {
    //input;
    int n;
    scanf("%d", &n);
    int year = n / 10000;
    bool find_HuiWen = false, find_ABAB = false;
    for (int i = year; i <= 9999; ++i) {
        string s_mmdd = to_string(i);
        reverse(s_mmdd.begin(), s_mmdd.end());
        if (!find_HuiWen && checkHuiWen(i, s_mmdd)) {
            string t = to_string(i) + s_mmdd;
            if (t <= to_string(n))
                continue;
            cout << t << endl;
            find_HuiWen = true;
        }
        if (!find_ABAB && check_ABAB(i) && checkHuiWen(i, s_mmdd)) {
            string t = to_string(i) + s_mmdd;
            if (t <= to_string(n))
                continue;
            cout << t << endl;
            find_ABAB = true;
        }
    }
    return 0;
}
```


[回到顶部](#目录)

---------------
## <a name="子串分值和">H: 子串分值和</a>

![子串分值和题目.jpg](https://i.loli.net/2021/04/15/2EmtZoN5q4enGgL.jpg)
【输入格式】  
输入一行包含一个由小写字母组成的字符串S 。  
【输出格式】  
输出一个整数表示答案。  
【样例输入】  
ababc  
【样例输出】  
28  
【样例说明】  
子串f值  

    a 1
    ab 2
    aba 2
    abab 2
    ababc 3
    b 1
    ba 2
    bab 2
    babc 3
    a 1
    ab 2
    abc 3
    b 1
    bc 2
    c 1

【评测用例规模与约定】  
对于20% 的评测用例，1 ≤ n ≤ 10；  
对于40% 的评测用例，1 ≤ n ≤ 100；  
对于50% 的评测用例，1 ≤ n ≤ 1000；  
对于60% 的评测用例，1 ≤ n ≤ 10000；  
对于所有评测用例，1 ≤ n ≤ 100000。  

### 分析
注意：此题解的思路**不是完美的**，大概只能通过样例的50%。  
要寻找所有的子串，我这里选择了两重for循环去枚举（**超时的原因**，暂时还没有想到更优的方案）。求出一段字符串中的一个字母出现的次数，我选择了前缀和作差。求出出现过多少种字母，就把每种字符从出现次数遍历一遍。  
实现：具体过程见代码和注释。  
时间复杂度为：O(n<sup>2</sup>\*26\*2) 。所以只能拿一半的分。


```c++
#include <cstdio>
#include <cstring>
#include <iostream>
#include <algorithm>

using namespace std;
#define input freopen("C:\\Users\\Administrator\\Desktop\\a.txt","r",stdin)
#define char_num 26

char s[100005];
int a[char_num][100005];  //a[i][j]表示字符串的第j个位置出现了第i个字母（第0个字母表示'a'）
int sum[char_num][100005];  //前缀和数组，sum[i][j]表示第j的位置以前第i个字母出现的次数

int getNum(int start, int end) {  //利用前缀和作差法求子串中含有字符的数量
    int cnt = 0;
    for (int i = 0; i < char_num; ++i)
        if (sum[i][end] - sum[i][start - 1] > 0)
            ++cnt;
    return cnt;
}

void print(int start, int end) {  //打印子串 （调试用）
    for (int i = start; i <= end; ++i) {
        putchar(s[i]);
    }
}

int main() {
//    input;
    scanf("%s", s + 1);
    int len = strlen(s + 1);
    for (int i = 1; i <= len; ++i) 
        ++a[s[i] - 'a'][i];
    for (int i = 0; i < char_num; ++i) 
        for (int j = 1; j <= len; ++j) 
            sum[i][j] = a[i][j] + sum[i][j - 1];
    long long ans = 0;
    for (int i = 1; i <= len; ++i) {  //枚举出所有可能出现的子串
        for (int j = i; j <= len; ++j) {
            int t = getNum(i, j);
            ans += t;
//            printf("当前子串:");
//            print(i, j);
//            printf("  当前得分: %d\n", t);
        }
    }
    printf("%lld", ans);
    return 0;
}
```

[回到顶部](#目录)

---------------
## <a name="平面切分">I: 平面切分</a>


[回到顶部](#目录)

---------------
## <a name="字串排序">J: 字串排序</a>

[回到顶部](#目录)

---------------








