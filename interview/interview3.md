### **今日头条2018校招后端方向（第三批）**

#### **编程题1**

题述：有一个推箱子的游戏, 一开始的情况如下图



![8537140-3](https://github.com/xingwy/Hugging-Algorithm/blob/master/images/8537140-3.png)

上图中, '.' 表示可到达的位置, '#' 表示不可到达的位置，其中 S 表示你起始的位置, 0表示初始箱子的位置, E表示预期箱子的位置，你可以走到箱子的上下左右任意一侧, 将箱子向另一侧推动。如下图将箱子向右推动一格; ..S0.. -> ...S0.注意不能将箱子推动到'#'上, 也不能将箱子推出边界;现在, 给你游戏的初始样子, 你需要输出最少几步能够完成游戏, 如果不能完成, 则输出-1。

输入描述：第一行为2个数字,n, m, 表示游戏盘面大小有n 行m 列(5< n, m < 50); 后面为n行字符串,每行字符串有m字符, 表示游戏盘面;

输出描述：一个数字,表示最少几步能完成游戏,如果不能,输出-1;

输入示例：3 6

​		   .S#..E 

​		   .#.0.. 

​		   ......

输出示例：11

```c++
#include <iostream>
#include <sstream>
#include <vector>
#include <string>
#include <algorithm>
#include <deque>
#include <memory.h>
#include <queue>
#include <functional>
using namespace std;


//改代码是示例代码*
int rows, cols;
vector<string> mat;
bool visited[50][50][50][50];
struct Step
{
	int manx, many;
	int boxx, boxy;
	int times;

	bool checkman()
	{
		return manx >= 0 && manx < cols && many >= 0 && many < rows && mat[many][manx] == '.';
	}

	bool checkbox()
	{
		return boxx >= 0 && boxx < cols && boxy >= 0 && boxy < rows && mat[boxy][boxx] == '.';
	}
	bool checkvisit()
	{
		if (visited[manx][many][boxx][boxy])
			return true;
		visited[manx][many][boxx][boxy] = true;
		return false;
	}
};

int main(int argc, char** argv) {
	cin >> rows >> cols;
	mat.resize(rows);
	int expectx, expecty;
	Step InitStep;
	for (size_t i = 0; i < rows; i++)
	{
		cin >> mat[i];
		for (size_t j = 0; j < cols; j++)
		{
			if (mat[i][j] == 'S') {
				InitStep.manx = j;
				InitStep.many = i;
				mat[i][j] = '.';
			}
			if (mat[i][j] == '0') {
				InitStep.boxx = j;
				InitStep.boxy = i;
				mat[i][j] = '.';
			}
			if (mat[i][j] == 'E') {
				expectx = j;
				expecty = i;
				mat[i][j] = '.';
			}
		}
	}
	InitStep.times = 0;
	int dirs[4][2] = {
		{ -1,0 },
		{ 0,1 },
		{ 1,0 },
		{ 0,-1 }
	};
	memset(visited, 0, 50 * 50 * 50 * 50);
	queue<Step> q;
	q.push(InitStep);
	int result = -1;
	while (result == -1 && !q.empty())
	{
		Step front = q.front();
		q.pop();
		for (size_t dir = 0; dir < 4; dir++)
		{
			//方向 左下右上
			Step nextStep = front;
			nextStep.times++;
			nextStep.manx += dirs[dir][0];
			nextStep.many += dirs[dir][1];
			if (!nextStep.checkman())
				continue;
			if (nextStep.manx == nextStep.boxx &&
				nextStep.many == nextStep.boxy) {
				nextStep.boxx += dirs[dir][0];
				nextStep.boxy += dirs[dir][1];
				if (!nextStep.checkbox())
					continue;
			}
			if (nextStep.checkvisit())
				continue;
			if (nextStep.boxx == expectx &&
				nextStep.boxy == expecty)
			{
				//cout << "找到"<<nextStep.times << endl;
				result = nextStep.times;
				break;
			}
			q.push(nextStep);
		}
	}
	cout << result << endl;
    return 0;
}
```



#### **编程题2**

题述：有n个房间，现在i号房间里的人需要被重新分配，分配的规则是这样的：先让i号房间里的人全都出来，接下来按照 i+1, i+2, i+3, ... 的顺序依此往这些房间里放一个人，n号房间的的下一个房间是1号房间，直到所有的人都被重新分配。

现在告诉你分配完后每个房间的人数以及最后一个人被分配的房间号x，你需要求出分配前每个房间的人数。数据保证一定有解，若有多解输出任意一个解。

输入描述：第一行两个整数n, x (2<=n<=10^5, 1<=x<=n)，代表房间房间数量以及最后一个人被分配的房间号； 第二行n个整数 a_i(0<=a_i<=10^9) ，代表每个房间分配后的人数。

输出描述：输出n个整数，代表每个房间分配前的人数。

输入示例：3 1 

​		   6 5 1

输出示例：4 4 4

```c++
#include<iostream>
#include<algorithm>
#include<vector>
using namespace std;

int main()      
{
	int n, k;
	cin >> n >> k;
	int min_i, min_n = 0x7fffffff;
	vector<int> vec(n+1);
	for (int i = 1; i <= n; i++) {
		cin >> vec[i];
		if (vec[i] <= min_n) {
			min_n = vec[i];
			min_i = i;
		}
	}
	for (int i = 1; i <= n; i++) {
		vec[i] -= min_n;
	}
	int tmp = (k < min_i ? min_i - k : k + n - min_i);
	for (int i = 0; i < tmp; i++) {
		vec[(k + n) % n]--;
	}
	vec[min_i] = n * min_n + tmp;
	for (int i = 1; i <= n; i++) {
		cout << vec[i] << " ";
	}
	return 0;
}
```

#### **附加题**

题述：二阶魔方又叫小魔方，是2*2*2的立方形结构。每一面都有4个块，共有24个块。每次操作可以将任意一面逆时针或者顺时针旋转90°，如将上面逆时针旋转90°操作如下。



![8537140-1](https://github.com/xingwy/Hugging-Algorithm/blob/master/images/8537140-1.png)

Nero在小魔方上做了一些改动，用数字替换每个块上面的颜色，称之为数字魔方。魔方上每一面的优美度就是这个面上4个数字的乘积，而魔方的总优美度就是6个面优美度总和。 现在Nero有一个数字魔方，他想知道这个魔方在操作不超过5次的前提下能达到的最大优美度是多少。 魔方展开后每一块的序号如下图：

![8537140-2](https://github.com/xingwy/Hugging-Algorithm/blob/master/images/8537140-2.png)

输入描述：输入一行包含24个数字，按序号顺序给出魔方每一块上面的数字。所有数大小范围为[-100,100]。

输出描述：输出一行包含一个数字，表示最大优美度。

输入示例：2 -3 -2 3 7 -6 -6 -7 9 -5 -9 -3 -2 1 4 -9 -1 -10 -5 -5 -10 -4 8 2

输出示例：8281

```c++
#include<iostream>
#include<algorithm>
#include<vector>
using namespace std;
int maxres = -0x7fffffff;
int p[6][4] = { {0,1,2,3},{4,5,10,11},{6,7,12,13},
				{8,9,14,15},{16,17,18,19},{20,21,22,23} };
//选三个互相垂直的面  记录每个面的顺时针与逆时针旋转矩阵
int rotates[6][24] = { {1,3,0,2,23,22,4,5,6,7,10,11,12,13,14,15,16,17,18,19,20,21,9,8},
					   {2,0,3,1,6,7,8,9,23,22,10,11,12,13,14,15,16,17,18,19,20,21,5,4},

					   {6,1,12,3,5,11,16,7,8,9,4,10,18,13,14,15,20,17,22,19,0,21,2,23},
					   {20,1,22,3,10,4,0,7,8,9,11,5,2,13,14,15,6,17,12,19,16,21,18,23},

					   {0,1,8,14,4,3,7,13,17,9,10,2,6,12,16,15,5,11,18,19,20,21,22,23},
					   {0,1,11,5,4,16,12,6,2,9,10,17,13,7,3,15,14,8,18,19,20,21,22,23} };
int getMaxCurRes(vector<int> v) {
	int sum = 0;
	for (int i = 0; i < 6; i++) {
		int res = 1;
		for (int j = 0; j < 4; j++) {

			res *= v[p[i][j]];
		}
		sum += res;
	}
	return sum;
}
void dfs(vector<int> v,int depth) {
	maxres = max(maxres, getMaxCurRes(v));
	if (depth == 5) {
		return;
	}
	else {
		for (int i = 0; i < 6; i++) {
			vector<int> tmp;
			for (int j = 0; j < v.size(); j++) {
				tmp.push_back(v[rotates[i][j]]);
			}
			dfs(tmp, depth + 1);
		}
	}
}
int main()      
{
	vector<int> vec(24);
	for (int i = 0; i < 24; i++) {
		cin >> vec[i];
	}
	dfs(vec, 0);
	cout << maxres;
	return 0;
}
```

