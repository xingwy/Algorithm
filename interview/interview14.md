### **腾讯2017暑期实习编程题**

#### **1、构造回文**

题述：给定一个字符串s，你可以从中删除一些字符，使得剩下的串是一个回文串。如何删除才能使得回文串最长呢？ 输出需要删除的字符个数。

输入描述：输入数据有多组，每组包含一个字符串s，且保证:1<=s.length<=1000。

输出描述：对于每组数据，输出一个整数，代表最少需要删除的字符个数。

输入示例：abcda google

输出示例：2 2

```c++
#include<iostream>
#include<algorithm>
#include<sstream>
#include<string>
#include<vector>
using namespace std;

int main()      
{
	//Needleman/Wunsch算法  求两个字符串的最大公共子串
	string str;
	while (cin >> str) {
		string str1(str);
		reverse(str1.begin(), str1.end());
		vector<vector<int>> res(str.size() + 1, vector<int>(str1.size() + 1, 0));
		for (int i = 1; i <= str.size(); i++) {
			for (int j = 1; j <= str1.size(); j++) {
				if (str[i - 1] == str1[j - 1]) {
					res[i][j] = res[i - 1][j - 1] + 1;
				}
				else {
					res[i][j] = max(res[i - 1][j], res[i][j - 1]);
				}
			}
		}
		cout << str.size() - res[str.size()][str1.size()];
	}
	return 0;
}
```

#### **2、字符移位**

题述：小Q最近遇到了一个难题：把一个字符串的大写字母放到字符串的后面，各个字符的相对位置不变，且不能申请额外的空间。 你能帮帮小Q吗？

输入描述：输入数据有多组，每组包含一个字符串s，且保证:1<=s.length<=1000。

输出描述：对于每组数据，输出移位后的字符串。

输入示例：AkleBiCeilD

输出示例：kleieilABCD

```c++
#include<iostream>
#include<algorithm>
#include<string>
using namespace std;

void swapChar(char &a,char &b) {
	a = a ^ b;     //a = a ^ b ^ b; b ^ b = 0
	b = a ^ b;
	a = a ^ b;
}
bool isA_Z(char a) {
	return a >= 'A' && a <= 'Z';
}
int main()      
{
	string str;
	while (cin >> str) {
		char *p = &str[0];
		while (*p) {
			while (*p && isA_Z(*p)) {
				p++;
			}
			while (*p && p >= &str[0] && isA_Z(*(p - 1))) {
				swapChar(*p, *(p - 1));
				p--;
			}
			p++;
		}
		cout << str;
	}
	return 0;
}
```

#### **3、有趣的数字**

题述：小Q今天在上厕所时想到了这个问题：有n个数，两两组成二元组，差最小的有多少对呢？差最大呢？

输入描述：输入包含多组测试数据。对于每组测试数据：N - 本组测试数据有n个数a1,a2...an - 需要计算的数据。保证:1<=N<=100000,0<=ai<=INT_MAX

输出描述：对于每组数据，输出两个数，第一个数表示差最小的对数，第二个数表示差最大的对数。

输入示例：6 45 12 45 32 5 6

输出示例：1 2

```c++
#include<iostream>
#include<algorithm>
#include<vector>
using namespace std;

int main()      
{
	int n;
	while (cin >> n) {
		int resMax = 0, resMin = 0;
		vector<int> vec(n);
		for (int i = 0; i < n; i++) {
			cin >> vec[i];
		}
		sort(vec.begin(), vec.end());
		int _min = vec[0], _max = vec[vec.size() - 1];
		if (_min == _max) {
			resMax = resMin = vec.size()*vec.size();
		}
		else {
			int l = 0, r = vec.size() - 1;
			while (vec[l] == _min) {
				l++;
			}
			while (vec[r] == _max) {
				r--;
			}
			resMax = l * (vec.size() - 1 - r);
			vector<int> tmp(n - 1);
			int item = 0;
			for (int i = 0; i < n - 1; i++) {
				tmp[i] = vec[i + 1] - vec[i];
				item = min(item, tmp[i]);
			}
			for (int i = 0; i < n - 1; i++) {
				if (tmp[i] == item) {
					resMin++;
				}
			}
			if (item == 0) {
				resMin = (resMin + 1)*resMin / 2;
			}
			else {
				cout << resMin;
			}
		}
		cout << resMin << " " << resMax;
	}
	return 0;
}
```

