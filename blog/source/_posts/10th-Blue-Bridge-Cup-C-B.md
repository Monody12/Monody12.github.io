title: 第十届蓝桥杯大赛软件类省赛 C/C++ 大学B 组
author: Monody12
tags:
  - simulation
  - Blue Bridge Cup
  - gcd
  - bfs
  - modulus
  - Base conversion
categories:
  - algorithm
date: 2021-04-04 21:46:00
---
## <a name="目录">目录</a>  
1. [组队](#组队)
2. [年号字串](#年号字串)
3. [数列求值](#数列求值)
4. [数的分解](#数的分解)
5. [迷宫](#迷宫)
6. [特别数的和](#特别数的和)
7. [完全二叉树的权值](#完全二叉树的权值)
8. [等差数列](#等差数列)
9. [后缀表达式](#后缀表达式)
10.[灵能传输](#灵能传输)


## <a name="组队">A: 组队 （递归回溯枚举）</a>
解法: 
	20个队员的5列全排列。
由于一个队员不能同时胜任两个位置，所以需要每次判断5个位置是否有同一个队员，需要使用vis数组

代码  
```C++
#include <iostream>
#include <algorithm>
#include <vector>

using namespace std;
int ans = 0;
bool vis[20];
int arr[20][5];
vector<int> path;

void print(vector<int> &a) {
    for (int &i:a)
        printf("%d ", i);
    printf("\n");
}

void dfs(int step, int n, int k) { //从n个数中选择k个
    if (path.size() == 5) { //比较这5个人的得分是否比较高
        int sum = 0;
        for (int i = 0; i < 5; ++i) {
            sum += arr[path[i]][i];
        }
        if (sum > ans) {
            ans = sum;
            for (int i = 0; i < path.size(); ++i) {
                printf("%d:%d  ", path[i], arr[path[i]][i]);
            }
            putchar(10);
        }
        // print(path);
        return;
    }
    for (int i = step; i < 20; ++i) {
        if (!vis[i]) {
            path.push_back(i);
            vis[i] = true;
            dfs(step + 1, n, k);
            vis[i] = false;
            path.pop_back();
        }
    }
}

int main() {
    int x;
    for (auto &i : arr) {
        scanf("%d", &x);
        for (int &j : i) {
            scanf("%d", &j);
        }
    }
    dfs(0, 20, 5);
    printf("%d", ans);
    return 0;
}
```

输出答案： 490  
[回到顶端](#目录)

-------

## <a name="年号字串">B: 年号字串 （进制转换）</a>

解法：
由题意知，十进制表示的数每增加1，字符串的末尾就前进一位，进满26位（也就是到‘Z’就向前一个字符进位）。所以这题可以看作是一个进制转换题，将10进制数转为26进制的字符串。 
注意：
这个26进制不是以'0'为起始的，而是以‘A’为起始的。

```C++
#include <iostream>
#include <algorithm>
#include <string>

using namespace std;

int main() {
    string s;
    int n = 2019;
    while (n) {
        s += n % 26 + 'A' - 1;
        n /= 26;
    }
    reverse(s.begin(), s.end());
    cout << s;
    return 0;
}
```
输出答案： BYQ  
[回到顶端](#目录)

--------
## <a name="">C: 数列求值 （模数）</a>
【问题描述】
给定数列1, 1, 1, 3, 5, 9, 17, …，从第4 项开始，每项都是前3 项的和。求
第20190324 项的最后4 位数字。

注意：这不是斐波那契数列！这是前三项和数列。  
解法：  
题外话：刚拿到题不知道为什么想到用大整数去计算，后面仔细一想，每往后2-5项就会是数列的最大值多一位。我试了一下BigInteger，还真爆内存了！  
正解：只用求20190324项的最后4位数字，那么数列的万位及以上我们就可以忽略不管，因为它们再怎么大也跟我想要的答案无关。所以当序列大于10000就取模就行。数列的递推过程类似斐波那契数列。

```java
public class Main {

    public static void main(String[] args) {
        int[] a = new int[20190324];
        a[1] = a[2] = a[0] = 1;
        for (int i = 3; i <= 20190323; i++) {
            a[i] = (a[i - 1] + a[i - 2] + a[i - 3]) % 10000;
        }
        System.out.println(a[20190323]);
    }
}

```

输出答案：4659  
[回到顶端](#目录)

--------


## <a name="数的分解">D: 数的分解 （模拟）</a>
【问题描述】  
把2019 分解成3 个各不相同的正整数之和，并且要求每个正整数都不包
含数字2 和4，一共有多少种不同的分解方法？  
注意交换3 个整数的顺序被视为同一种方法，例如1000+1001+18 和
1001+1000+18 被视为同一种。

```C++
#include <algorithm>
#include <vector>
#include <set>

#define input freopen("C:\\Users\\Administrator\\Desktop\\a.txt","r",stdin)

using namespace std;

bool check(int n) { //检查整数中是否不包含2或4这两个数字
    if (n <= 0 || n >= 2019)
        return false;
    while (n) {
        int t = n % 10;
        if (t == 2 || t == 4)
            return false;
        n /= 10;
    }
    return true;
}

int main() {
    //input;
    vector<int> nums;
    for (int i = 1; i <= 2017; ++i) { //筛选出能用的数字, 筛选后只有1000多个数字了
        if (check(i))
            nums.push_back(i);
    }
    set<set<int>> ans;
    for (int i = 0; i < nums.size(); ++i) { //双重循环来枚举可以出现的组合, 并使用set集合来去除重复的组合
        for (int j = i + 1; j < nums.size(); ++j) {
            int t = 2019 - nums[i] - nums[j];
            if (check(t) && t != nums[i] && t != nums[j]) {
                ans.insert({t, nums[i], nums[j]});
            }
        }
    }
    printf("总共有%d个组合", ans.size());
    return 0;
}
```
输出答案：40785  
[回到顶端](#目录)

--------

## <a name="迷宫">E: 迷宫 （BFS）</a>
【问题描述】
下图给出了一个迷宫的平面图，其中标记为1 的为障碍，标记为0 的为可
以通行的地方。
```
010000
000100
001001
110000
```
迷宫的入口为左上角，出口为右下角，在迷宫中，只能从一个位置走到这
个它的上、下、左、右四个方向之一。  
对于上面的迷宫，从入口开始，可以按`DRRURRDDDR` 的顺序通过迷宫，
一共10 步。其中D、U、L、R 分别表示向下、向上、向左、向右走。
对于下面这个更复杂的迷宫（30 行50 列），请找出一种通过迷宫的方式，
其使用的步数最少，在步数最少的前提下，请找出字典序最小的一个作为答案。
请注意在字典序中D<L<R<U。（如果你把以下文字复制到文本文件中，请务
必检查复制的内容是否与文档中的一致。
```
01010101001011001001010110010110100100001000101010
00001000100000101010010000100000001001100110100101
01111011010010001000001101001011100011000000010000
01000000001010100011010000101000001010101011001011
00011111000000101000010010100010100000101100000000
11001000110101000010101100011010011010101011110111
00011011010101001001001010000001000101001110000000
10100000101000100110101010111110011000010000111010
00111000001010100001100010000001000101001100001001
11000110100001110010001001010101010101010001101000
00010000100100000101001010101110100010101010000101
11100100101001001000010000010101010100100100010100
00000010000000101011001111010001100000101010100011
10101010011100001000011000010110011110110100001000
10101010100001101010100101000010100000111011101001
10000000101100010000101100101101001011100000000100
10101001000000010100100001000100000100011110101001
00101001010101101001010100011010101101110000110101
11001010000100001100000010100101000001000111000010
00001000110000110101101000000100101001001000011101
10100101000101000000001110110010110101101010100001
00101000010000110101010000100010001001000100010101
10100001000110010001000010101001010101011111010010
00000100101000000110010100101001000001000000000010
11010000001001110111001001000011101001011011101000
00000110100010001000100000001000011101000000110011
10101000101000100010001111100010101001010000001000
10000010100101001010110000000100101010001011101000
00111100001000010000000110111000000001000000001011
10000001100111010111010001000110111010101101111000
```

这是一道传统的广搜题，但是在做这题的时候遇到了一个很迷的问题：如果把点是否走过的标记数组vis的赋值放在while循环中，就会和答案不一致，而放在方向循环中（如下）就得到的是正确的答案。  
正确代码：
```C++
#include <algorithm>
#include <iostream>
#include <vector>
#include <queue>
#include <cstdlib>
#include <string>

#define input freopen("C:\\Users\\Administrator\\Desktop\\a.txt","r",stdin)

using namespace std;

string s[31][51];
string c[] = {"D", "L", "R", "U"};
char map[31][51];
bool vis[31][51];
int n, m, dr[4][2]{{1,  0},
                   {0,  -1},
                   {0,  1},
                   {-1, 0}};

string bfs() {
    queue<pair<int, int>> q;
    q.push({1, 1});
    vis[1][1] = true;
    while (!q.empty()) {
        int x = q.front().first, y = q.front().second;
        q.pop();
        for (int i = 0; i < 4; ++i) {
            int dx = x + dr[i][0], dy = y + dr[i][1];
            if (dx < 1 || dy < 1 || dx > n || dy > m || vis[dx][dy] || map[dx][dy] == '1')
                continue;
            q.push({dx, dy});
            vis[dx][dy] = true;
            s[dx][dy] = s[x][y] + c[i];
            if (dx == n && dy == m)
                return s[dx][dy];
        }
    }
    return "找不到终点";
}

int main() {
    //input;
    //scanf("%d%d", &n, &m);
    n = 30, m = 50;
    for (int i = 1; i <= n; ++i) {
        scanf("%s", map[i] + 1);
    }
    string ans = bfs();
    cout << ans;
    return 0;
}
```

输出答案：
```
DDDDRRURRRRRRDRRRRDDDLDDRDDDDDDDDDDDDRDDRRRURRUURRDDDDRDRRRRRRDRRURRDDDRRRRUURUUUUUUULULLUUUURRRRUULLLUUUULLUUULUURRURRURURRRDDRRRRRDDRRDDLLLDDRRDDRDDLDDDLLDDLLLDLDDDLDDRRRRRRRRRDDDDDDRR
```
[回到顶端](#目录)

------
## <a name="特别数的和">F: 特别数的和 (模拟)</a>
【问题描述】  
小明对数位中含有2、0、1、9 的数字很感兴趣（不包括前导0），在1 到
40 中这样的数包括1、2、9、10 至32、39 和40，共28 个，他们的和是574。
请问，在1 到n 中，所有这样的数的和是多少？  

【样例输入】  
40  
【样例输出】  
574  

```c++
#include <algorithm>
#include <iostream>
#define input freopen("C:\\Users\\Administrator\\Desktop\\a.txt","r",stdin)

using namespace std;

bool check(int n) {
    while (n) {
        int t = n % 10;
        if (t == 2 || t == 0 || t == 1 || t == 9)
            return true;
        n /= 10;
    }
    return false;
}

int main() {
    //input;
    int n, sum = 0;
    scanf("%d", &n);
    for (int i = 0; i <= n; ++i) {
        if (check(i))
            sum += i;
    }
    printf("%d", sum);
    return 0;
}
```
[回到顶端](#目录)

 -------

## <a name="完全二叉树的权值">G: 完全二叉树的权值 (bfs)</a>
![完全二叉树的权值题目.jpg](https://i.loli.net/2021/04/05/9FYlzXQrsm6KN5x.jpg)

解法：
存储方法：由于题中的树是一颗完全二叉树，所以可以将其存入数组。根节点下标为1。当前节点的左子树节点下标为当前节点下标`index * 2`，右子树为`index * 2 + 1`   
遍历方法：以根节点为起点进行层序遍历（BFS 广度优先遍历），每次可以遍历一层。
```c++
#include <cstdio>
#include <climits>
#include <queue>

using namespace std;

int tree[100005];

int main() {
    int n;
    scanf("%d", &n);
    for (int i = 1; i <= n; ++i) {
        scanf("%d", tree + i);
    }
    queue<int> level;
    level.push(1);
    int max_value_level = 1, this_level = 0;
    long long max_value_sum = LLONG_MIN;
    while (!level.empty()) {
        ++this_level;
        long long sum = 0; //存储本层节点值的和
        queue<int> node;
        while (!level.empty()) { //将本层二叉树的节点下标存入node队列中
            node.push(level.front());
            level.pop();
        }
        while (!node.empty()) {
            int t = node.front();
            node.pop();
            sum += tree[t];
            if (t * 2 + 1 < n) {
                level.push(t * 2);
                level.push(t * 2 + 1);
            }
        }
        if (sum > max_value_sum) {
            max_value_sum = sum;
            max_value_level = this_level;
        }
    }
    printf("%d", max_value_level);
    return 0;
}

```
[回到顶端](#目录)

----

## <a name="等差数列">H: 等差数列 （最大公因数）</a>

【问题描述】  
数学老师给小明出了一道等差数列求和的题目。但是粗心的小明忘记了一
部分的数列，只记得其中N 个整数。   
现在给出这N 个整数，小明想知道包含这N个整数的最短的等差数列有几项？  
【样例输入】  
5  
2 6 4 10 20  
【样例输出】  
10  
【样例说明】  
包含2、6、4、10、20 的最短的等差数列是2、4、6、8、10、12、14、16、
18、20。

### 解法:
要解决这个问题，就必须要知道这个等差数列的公差。  
这是一个数学问题，这里就直接说结论了。把当前序列进行排序，求出相邻两个数的差。求出这些差的最小公倍数即是这个等差数列最大可能出现的公差。

```c++
#include <algorithm>
#include <iostream>
#include <climits>

#define input freopen("C:\\Users\\Administrator\\Desktop\\a.txt","r",stdin)

using namespace std;

int a[100005];

int gcd(int a, int b) {
    return b ? gcd(b, a % b) : a;
}

int main() {
    input;
    int n;
    scanf("%d", &n);
    for (int i = 0; i < n; ++i) {
        scanf("%d", a + i);
    }
    sort(a, a + n); //升序排序
    int dif = a[1] - a[0];
    for (int i = 1; i < n; ++i) {
        dif = gcd(dif, a[i] - a[i - 1]); //求出当前序列的所有的差值, 求出它们的最大公因数就是最大可能出现的公差
    }
    printf("%d\n", (a[n - 1] - a[0]) / dif + 1);
    return 0;
}
```
[回到顶端](#目录)

----

