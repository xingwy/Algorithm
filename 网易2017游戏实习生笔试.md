### **2017网易游戏实习生招聘笔试**

#### **1、字符串编码**

题述：给定一个字符串，请你将字符串重新编码，将连续的字符替换成“连续出现的个数+字符”。比如字符串AAAABCCDAA会被编码成4A1B2C1D2A。 

输入描述：每个测试输入包含1个测试用例 每个测试用例输入只有一行字符串，字符串只包括大写英文字母，长度不超过10000。

输出描述：输出编码后的字符串

输入示例：AAAABCCDAA

输出示例：4A1B2C1D2A

```c++
#include<iostream>
#include <algorithm>
#include <sstream>
#include <string>
using namespace std;

int main()      
{
	string str, res;
	cin >> str;
	int count = 1;
	char ch = str[0];
	for (int i = 1; i < str.size(); i++) {
		if (ch == str[i]) {
			count++;
			continue;
		}
		else {
			stringstream streamitos;
			streamitos << count;
			string tmp = streamitos.str();
			res += tmp;
			res.push_back(ch);
			count = 1;
			ch = str[i];
		}
	}
	cout << res;
	return 0;
}
```

#### **2、最大和**

题述：在一个N*N的数组中寻找所有横，竖，左上到右下，右上到左下，四种方向的直线连续D个数字的和里面最大的值。

输入描述：每个测试输入包含1个测试用例，第一行包括两个整数 N 和 D : 3 <= N <= 100 1 <= D <= N 接下来有N行，每行N个数字d: 0 <= d <= 100。

输出描述：输出一个整数，表示找到的和的最大值

输入示例：4 2 

​		   87 98 79 61 

​                   10 27 95 70 

​                   20 64 73 29 

​                   71 65 15 0

输出示例：193

```c++
#include<iostream>
#include <algorithm>
#include <vector>
using namespace std;

int getMaxConnectSum(vector<int> p,int n,int d) {
	int maxNum = 0;
	if (p.size() < d) {
		for (int i = 0; i < p.size(); i++) {
			maxNum += p[i];
		}
		return maxNum;
	}
	for (int i = 0; i <= p.size() - d; i++) {
		int tmp = 0;
		for (int j = 0; j < d; j++) {
			tmp += p[i + j];
		}
		maxNum = max(maxNum, tmp);
		tmp = 0;
	}
	return maxNum;
}
int main()      
{
	int n, d;
	cin >> n >> d;
	int p[105][105];
	for (int i = 0; i < n*n; i++) {
		cin >> p[i / n][i % n];
	}
	vector<vector<int>> vec;
	//添加横纵的数组
	for (int i = 0; i < n; i++) {
		vector<int> tmp1, tmp2;
		for (int j = 0; j < n; j++) {
			tmp1.push_back(p[i][j]);
			tmp2.push_back(p[j][i]);
		}
		vec.push_back(tmp1);
		vec.push_back(tmp2);
	}
	//添加斜角数组
	for (int i = 0; i < n; i++) {
		vector<int> tmp1, tmp2, tmp3, tmp4;
		for (int j = 0; j + i < n; j++) {
			tmp1.push_back(p[j + i][j]);
			tmp2.push_back(p[j][j + i]);

			tmp3.push_back(p[j+i][n-1-j]);
			tmp4.push_back(p[j][n-1-j - i]);
		}
		vec.push_back(tmp1);
		vec.push_back(tmp2);
		vec.push_back(tmp3);
		vec.push_back(tmp4);
	}
	int res = 0;
	for (int i = 0; i < vec.size(); i++) {
		res = max(res, getMaxConnectSum(vec[i], n, d));
	}
	cout << res;
	return 0;
}
```

#### **3、推箱子**

题述：大家一定玩过“推箱子”这个经典的游戏。具体规则就是在一个N*M的地图上，有1个玩家、1个箱子、1个目的地以及若干障碍，其余是空地。玩家可以往上下左右4个方向移动，但是不能移动出地图或者移动到障碍里去。如果往这个方向移动推到了箱子，箱子也会按这个方向移动一格，当然，箱子也不能被推出地图或推到障碍里。当箱子被推到目的地以后，游戏目标达成。现在告诉你游戏开始是初始的地图布局，请你求出玩家最少需要移动多少步才能够将游戏目标达成。

输入描述：每个测试输入包含1个测试用例 第一行输入两个数字N，M表示地图的大小。其中0<N，M<=8。 接下来有N行，每行包含M个字符表示该行地图。其中 . 表示空地、X表示玩家、*表示箱子、#表示障碍、@表示目的地。 每个地图必定包含1个玩家、1个箱子、1个目的地。

输出描述：输出一个数字表示玩家最少需要移动多少步才能将游戏目标达成。当无论如何达成不了的时候，输出-1。

输入示例：4 4 

​		   .    .    .    . 

​		   .    .   *   @

​                   .    .    .    . 

​                   .    X   .    . 

​                   6 6 

​                   .    .    .    #   .    . 

​                   .    .    .    .    .    .

​                  #   *   #   #   .    .

​                  .     .   #   #   .    #

​                  .     .   X    .    .    . 

​                  .    @  #    .    .    .

输出示例：3 11

```c++
//bfs算法，后面更新出相应代码
```

#### **4、赛马**

题述：在一条无限长的跑道上，有N匹马在不同的位置上出发开始赛马。当开始赛马比赛后，所有的马开始以自己的速度一直匀速前进。每匹马的速度都不一样，且全部是同样的均匀随机分布。在比赛中当某匹马追上了前面的某匹马时，被追上的马就出局。 请问按以上的规则比赛无限长的时间后，赛道上剩余的马匹数量的数学期望是多少

输入描述：每个测试输入包含1个测试用例 输入只有一行，一个正整数N 1 <= N <= 1000

输出描述：输出一个浮点数，精确到小数点后四位数字，表示剩余马匹数量的数学期望

输入示例：1 2

输出示例：1.0000 1.5000...00

```c++
#include<iostream>
using namespace std;

int main()      
{
	/*这是一道数学题,以p[i]代表i匹马存活的概率，那我们能很容易想到最后的结果
	// E(i) = p[1]*1 + p[2]*2 + p[3]*3 +··· + p[n]*n 。这样的求出每个P[i]
	//的概率。比较麻烦。换个思路，如果将一匹马的存活看做两个状态。存or活，那么
	//这匹马所有存在的所有概率将包含在p[1]到p[n]中 且贡献值为1。
	//那么我们求出每匹马存活的概率值的和也就等价于E(i)了
	*/
	double n, res = 0;
	cin >> n;
	for (double i = 1; i <= n; i++) {
		res += 1 / i;
	}
	printf("%.4f", res);
	return 0;
}
```

























