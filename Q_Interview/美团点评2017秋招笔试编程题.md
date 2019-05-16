### **美团点评2017秋招笔试编程题**

#### **1、大富翁游戏**

题述：大富翁游戏，玩家根据骰子的点数决定走的步数，即骰子点数为1时可以走一步，点数为2时可以走两步，点数为n时可以走n步。求玩家走到第n步（n<=骰子最大点数且是方法的唯一入参）时，总共有多少种投骰子的方法。

输入描述：输入包括一个整数n,(1 ≤ n ≤ 6)

输出描述：输出一个整数,表示投骰子的方法

输入示例：6

输出示例：32

```c++
#include<iostream>
#include<algorithm>
using namespace std;

int main()      
{
	//先说思路 设为f(n)为玩家走到n所需要的步数，对于f(n+1),可以先走到n处再走1步到达n+1,
	//也可以走到n-1处，再走2步到n+1,类推可以走到i处，再走n-i步到n+1处。
	//即可得到 f(n+1)=S(n)+1,(S(n)=f(n)+f(n-1)+f(n-2)+..+f(1))。
	//①f(n+1)=S(n)+1					
	//②f(n)=S(n-1)+1
	//联立①②得f(n+1)-f(n)=f(n)  => f(n+1)=2f(n)
	int n;
	cin >> n;
	long long res = 1;
	cout << (res << (n - 1));
	return 0;
}
```

#### **2、拼凑纸币**

题述：给你六种面额 1、5、10、20、50、100 元的纸币，假设每种币值的数量都足够多，编写程序求组成N元（N为0~10000的非负整数）的不同组合的个数。

输入描述：输入包括一个整数n(1 ≤ n ≤ 10000)

输出描述：输出一个整数,表示不同的组合方案数

输入示例：1

输出示例：1

```c++
#include<iostream>
#include<algorithm>
#include<vector>
using namespace std;

int main()      
{
	//依次计算 只使用面值1,只使用面值1、5，只使用面值1、5、10...的情况种数。
	int n;
	cin >> n;
	vector<long long> res(n + 1, 0);
	int coin[6] = { 1,5,10,20,50,100 };
	res[0] = 1;
	for (int i = 0; i < 6; i++) {
		for (int j = 1; j <= n; j++) {
			if (j >= coin[i]) {
				res[j] += res[j - coin[i]];
			}
		}
	}
	cout << res[n];
	return 0;
}
```

#### **3、最大矩形面积**

题述：给定一组非负整数组成的数组h，代表一组柱状图的高度，其中每个柱子的宽度都为1。 在这组柱状图中找到能组成的最大矩形的面积（如图所示）。

![5583018-1](https://github.com/xingwy/Hugging-Algorithm/blob/master/images/5583018-1.png)

入参h为一个整型数组，代表每个柱子的高度，返回面积的值。

输入描述：输入包括两行,第一行包含一个整数n(1 ≤ n ≤ 10000) 第二行包括n个整数,表示h数组中的每个值,h_i(1 ≤ h_i ≤ 1,000,000)

输出描述：输出一个整数,表示最大的矩阵面积。

输入示例：6 

​		   2 1 5 6 2 3

输出示例：10

```c++
#include<iostream>
#include<algorithm>
#include<vector>
using namespace std;

int main()      
{
	//思路分析： 其实就是在给定的数组中  求一个子数组满足子数组长度len,最小值min
	//min*len最大值。 可以选取每一个数组元素作为子数组最小值求解。
	int n;
	cin >> n;
	vector<int> vec(n);
	for (int i = 0; i < n; i++) {
		cin >> vec[i];
	}
	int max_area = 0;
	for (int i = 0; i < n; i++) {
		int l = i, r = i;
		while (true) {
			if (l >= 1) {
				if (vec[l - 1] >= vec[i]) {
					l--;
					continue;
				}
				break;
			}
			break;
		}
		while (true) {
			if (r <= n - 2) {
				if (vec[r + 1] >= vec[i]) {
					r++;
					continue;
				}
				break;
			}
			break;
		}
		max_area = max(max_area, vec[i] * (r - l + 1));
	}
	cout << max_area;
	return 0;
}
```

#### **4、最长公共连续子串**

题述：给出两个字符串（可能包含空格）,找出其中最长的公共连续子串,输出其长度。

输入描述：输入为两行字符串（可能包含空格），长度均小于等于50.

输出描述：输出为一个整数，表示最长公共连续子串的长度。

输入示例：abcde abgde

输出示例：2

```c++
#include<iostream>
#include<algorithm>
#include<vector>
using namespace std;

int main()      
{
	string str1, str2;
	cin >> str1 >> str2;
	vector<vector<int>> dp(str1.size() + 1, vector<int>(str2.size() + 1, 0));
	int res = 0;
	for (int i = 1; i < str1.size(); i++) {
		for (int j = 1; j < str2.size(); j++) {
			if (str1[i - 1] == str2[j - 1]) {
				dp[i][j] = dp[i - 1][j - 1] + 1;
			}
			else {
				dp[i][j] = 0;
			}
			res = max(dp[i][j], res);
		}
	}
	cout << res;
	return 0;
}
```

