title: 进制转换
author: Monody12
date: 2021-04-04 22:00:10
tags:
---
## 进制转换
#### 蓝桥竞赛必不可少的的算法之一
### 十进制转任意进制代码模板

```C++
	void mystrrev(char *s) {//我的字符串反转函数
	int len = strlen(s);
    for (int i = 0; i < len / 2; ++i)
		swap(s[i], s[len - i - 1]);
	}
	void myitoa(int n, char *s, int radix = 10) {//我的进制转换存入字符串函数
	int index = 0;
	do {
		int t = n % radix;
		if (t >= 0 && t <= 9) s[index++] = t + '0';
		else s[index++] = t - 10 + 'A';//题目要求16进制为大写就是A
		n /= radix;
	 } while (n != 0);//使用do{}while（）以防止输入为0的情况
		s[index] = '\0';
		//strrev(s);//字符串反转   oj用不了strrev只能手写
		mystrrev(s);
	}


```