title: 链表
author: Monody12
date: 2021-08-01 12:19:49
tags:
---
## Friends and Gifts （链表 环 图）

### 题意

在一群人中，需要互相送礼物，自己不能给自己送礼物，每个人必须发送和接收一份礼物。题目给定了部分方案数组f，即数组第i个人发送礼物给第f[i]个人，总共有n人。当f[i]=0时表示未决定送礼物的人，要我们把这些0换成合适的数。

### 分析  
每个人的发送和接收礼物都是1对1的关系，并且不能是自身对应自身的自环。而且题中的每个人的礼物接收情况未确定，我们分类讨论一下。
1. 既没有收到，又发送了。
2. 收到了，但没有发送。
3. 没有收到，但发送了。
4. 既收到了，又发送了。

其中，状态4已经被题目确定了，我们也不能更改。状态1是最自由的，可以任由我们操作。  
此时，以上关系我们可以使用**双向循环链表**来描述。
状态1对应零散的单节点，需要我们插入链表。状态2对应链表表尾，状态3对应链表表头，状态4对应链表的中间节点。

### 实现
我们需要将题中的点全部用链表连接起来。其实没必要使用双向链表的，使用只是为了更好理解题目人与人之间的关系而已。  
步骤
1. 收集每个点的发送与接收关系，用send个get两个数组就可以实现。
2. 根据以上两个数组，将表头和单节点分类出来以供我们使用。这里我用队列来存储的。
3. 从找到的第一个链表（在此链表表示未形成环的链表，因为形成环的就满足题意了，不需要我们操作了）从表头开始，遍历到链表尾，观察是否有自由的单节点，若有就将所有的单节点依次插入到当前链表表尾。
4. 此时查看是否还存在其余未成环的链表，如果有，就将这个链表表尾连接到下一个链表的表头。若无就将这个链表表尾连接到**第一个**遍历到的链表的表头（形成一个循环链表）。
5. 至此，每一个人都有发送和接收了，这就是循环链表。

注意一些细节：
+ 题中给出的条件中我们能够连出多个循环链表吗？  
根据我们的思想，从单链表开始一路连接，只能形成一个循环链表，除非题目中已经有相互之间分配好的状况（也就是一开始就存在的循环链表），当然，这些已经存在的循环链表节点我们在步骤二的时候就排除掉了，自己不会搜到。
+ 当题中不存在链表只存在单节点时怎么办？  
我们拿一个单节点出来形成一个单链表就行。

### 代码  

```c++
#include <cstdio>
#include <queue>

#define input freopen("C:\\Users\\Administrator\\Desktop\\a.txt","r",stdin)

using namespace std;

const int maxn = 2e5 + 5;

int send[maxn], get[maxn];

void classify(queue<int> &head, queue<int> &node, int n) {
    for (int i = 1; i <= n; ++i) {
        if (!send[i] && !::get[i])  //单节点
            node.push(i);
        else if (send[i] && !::get[i])  //链表表头
            head.push(i);
    }
}

int main(int argc, char *argv[]) {
//    input;
    int n;
    scanf("%d", &n);
    for (int i = 1; i <= n; ++i) {
        scanf("%d", &send[i]);
        ::get[send[i]] = i;
    }
    queue<int> head, node;
    classify(head, node, n);
    int head_node = 0, tail_node = 0;  //head_node为全局表头，tail_node为当前尾指针
    if (head.empty()&&!node.empty()){  //如果链表表头为空，就拿一个单节点充当链表表头
        head.push(node.front());
        node.pop();
    }
    while (!head.empty()) {
        if (head_node == 0)
            head_node = head.front();
        tail_node = head.front();
        head.pop();
        while (send[tail_node])  //遍历到链表尾
            tail_node = send[tail_node];
        while (!node.empty()) {
            int x = node.front();  //取出一个新的单节点
            node.pop();
            send[tail_node] = x;
            ::get[x] = tail_node;
            tail_node = x;
        }
        if (!head.empty())  //链表表头非空
            send[tail_node] = head.front();
        else{
            send[tail_node] = head_node;
        }
    }
    for (int i = 1; i <= n; ++i) {
        printf("%d ", send[i]);
    }
    printf("\n");

    return 0;
}
```