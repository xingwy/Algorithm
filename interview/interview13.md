### **网易2018校园招聘编程题**

#### **1、魔法币**

题述：小易准备去魔法王国采购魔法神器,购买魔法神器需要使用魔法币,但是小易现在一枚魔法币都没有,但是小易有两台魔法机器可以通过投入x(x可以为0)个魔法币产生更多的魔法币。 魔法机器1:如果投入x个魔法币,魔法机器会将其变为2x+1个魔法币 魔法机器2:如果投入x个魔法币,魔法机器会将其变为2x+2个魔法币 小易采购魔法神器总共需要n个魔法币,所以小易只能通过两台魔法机器产生恰好n个魔法币,小易需要你帮他设计一个投入方案使他最后恰好拥有n个魔法币。 输入描述: 输入包括一行,包括一个正整数n(1 ≤ n ≤ 10^9),表示小易需要的魔法币数量。

输出描述：输出一个字符串，每个字符表示该次小易选取的魔法器。其中只包含字符'1'和'2'。

输入例子：10

输出例子：122

```c++
#include <iostream>
#include <algorithm>
#include <string>
using namespace std;

int main() {
	int n;
	cin >> n;
	string str;
	while (n > 0) {
		if (n % 2) {
			n = (n - 1) / 2;
			str.push_back('1');
		}
		else {
			n = (n - 2) / 2;
			str.push_back('2');
		}
	}
	reverse(str.begin(), str.end());
	cout << str;
	return 0;
}
```

#### **2、相反数**

题述：为了得到一个数的"相反数",我们将这个数的数字顺序颠倒,然后再加上原先的数得到"相反数"。例如,为了得到1325的"相反数",首先我们将该数的数字顺序颠倒,我们得到5231,之后再加上原先的数,我们得到5231+1325=6556.如果颠倒之后的数字有前缀零,前缀零将会被忽略。例如n = 100, 颠倒之后是1. 输入描述: 输入包括一个整数n,(1 ≤ n ≤ 10^5)

输出描述：输出一个整数,表示n的相反数

输入例子：1325

输出例子：6556

```c++
#include <iostream>
#include <algorithm>
#include <sstream>
using namespace std;

int main() {
	int n;
	cin >> n;
	string str;
	int item;
	stringstream stream_itos;
	stream_itos << n;
	str = stream_itos.str();
	reverse(str.begin(), str.end());
	stringstream stream_stoi(str);
	stream_stoi >> item;
	cout << n + item;
	return 0;
}
```

#### **3、字符串碎片**

题述：一个由小写字母组成的字符串可以看成一些同一字母的最大碎片组成的。例如,"aaabbaaac"是由下面碎片组成的:'aaa','bb','c'。牛牛现在给定一个字符串,请你帮助计算这个字符串的所有碎片的平均长度是多少。

输入描述：输入包括一个字符串s,字符串s的长度length(1 ≤ length ≤ 50),s只含小写字母('a'-'z')

输出描述：输出一个整数,表示所有碎片的平均长度,四舍五入保留两位小数。

如样例所示: s = "aaabbaaac" 所有碎片的平均长度 = (3 + 2 + 3 + 1) / 4 = 2.25

输入例子1: aaabbaaac

输出例子1: 2.25

```c++
#include <iostream>
#include <string>
using namespace std;

int main() {
	string str;
	cin >> str;
	double sum = 0, count, itemNum = 0;
	char item;
	for (int i = 0; i < str.length(); i++) {
		item = str[i];
		int j = i+1;
		count = 1;
		while (str[j] == item) {
			count++;
			j++;
		}
		i = j-1;
		itemNum++;
		sum += count;
	}
	cout << sum / itemNum << endl;
	return 0;
}
```

#### **4、游历魔法王国**

题述：魔法王国一共有n个城市,编号为0~n-1号,n个城市之间的道路连接起来恰好构成一棵树。 小易现在在0号城市,每次行动小易会从当前所在的城市走到与其相邻的一个城市,小易最多能行动L次。 如果小易到达过某个城市就视为小易游历过这个城市了,小易现在要制定好的旅游计划使他能游历最多的城市,请你帮他计算一下他最多能游历过多少个城市(注意0号城市已经游历了,游历过的城市不重复计算)。 输入描述: 输入包括两行,第一行包括两个正整数n(2 ≤ n ≤ 50)和L(1 ≤ L ≤ 100),表示城市个数和小易能行动的次数。 第二行包括n-1个整数parent[i](0 ≤ parent[i] ≤ i), 对于每个合法的i(0 ≤ i ≤ n - 2),在(i+1)号城市和parent[i]间有一条道路连接。

输出描述: 输出一个整数,表示小易最多能游历的城市数量。

输入例子1: 5 2 0 1 2 3

输出例子1: 3

```c++
#include <iostream>
#include <algorithm>
#include <list>
#include <vector>
using namespace std;
typedef list<int> node;
void dps(int par, int depth, node* p, vector<int>& res) {
	if (p[par].size() == 0) {
		res.push_back(depth);
		return;
	}
	list<int>::iterator it;
	for (it = p[par].begin(); it != p[par].end(); it++) {
		dps(*it, depth + 1, p, res);
	}
}
int main() {            //基本思路  求出深度表 =》优先后选最深节点
	int N, L, count;
	cin >> N >> L;
	node ptr[50];
	for (int i = 0; i < N - 1; i++) {
		int item;
		cin >> item;
		ptr[item].push_back(i + 1);
	}
	vector<int> res;
	dps(0, 0, ptr, res);
	sort(res.begin(), res.end(), greater<int>());
    
	if (L < res[0]) {
		count = L + 1;
	}
	else {
		L -= res[0];
		int numAll = 0;
		for (int i = 1; i < res.size(); i++) {
			numAll += res[i];
		}
		if (L >= 2 * numAll) {
			count = res[0] + numAll + 1;
		}
		else {
			count = res[0] + L / 2 + 1;
		}
	}
	cout << count << endl;
	return 0;
}
```

#### **5、重排数列**

题述：小易有一个长度为N的正整数数列A = {A[1], A[2], A[3]..., A[N]}。 牛博士给小易出了一个难题: 对数列A进行重新排列,使数列A满足所有的A[i] * A[i+1] (1 ≤ i ≤ N - 1)都是4的倍数。 小易现在需要判断一个数列是否可以重排之后满足牛博士的要求。 输入描述: 输入的第一行为数列的个数t(1 ≤ t ≤ 10), 接下来每两行描述一个数列A,第一行为数列长度n(1 ≤ n ≤ 10^5) 第二行为n个正整数A[i] (1 ≤ A[i] ≤ 10^9)

输出描述：对于每个数列输出一行表示是否可以满足牛博士要求,如果可以输出Yes,否则输出No。

输入例子1：

2 

3

1 10 100 

4

1 2 3 4

输入例子1：

Yes 

No

```c++
#include <iostream>
#include <string>
#include <vector>
using namespace std;

int main() {
	//基本思路  求%2  %4 及非此数据的个数比
	int t;
	cin >> t;
	vector<int> data[10];
	string result[10];
	for (int i = 0; i < t; i++) {
		int n;
		cin >> n;
		for (int j = 0; j < n; j++) {
			int item;
			cin >> item;
			data[i].push_back(item);
		}
		int res[3] = { 0,0,0 };
		for (int j = 0; j < data[i].size(); j++) {
			if (data[i][j] % 4 == 0) {
				res[0]++;
			}
			else if (data[i][j] % 2 == 0) {
				res[1]++;
			}
			else {
				res[2]++;
			}
		}
		if (res[2] <= (res[0] + (res[2] == 0)))
			result[i] = "Yes";
		else
			result[i] = "No";
	}
	for (int i = 0; i < t; i++) {
		cout << result[i] << endl;
	}
	return 0;
}
```

#### **6、最长公共子括号序列**

题述：一个合法的括号匹配序列被定义为:

1. 空串""是合法的括号序列
2. 如果"X"和"Y"是合法的序列,那么"XY"也是一个合法的括号序列
3. 如果"X"是一个合法的序列,那么"(X)"也是一个合法的括号序列
4. 每个合法的括号序列都可以由上面的规则生成 例如"", "()", "()()()", "(()())", "(((()))"都是合法的。 从一个字符串S中移除零个或者多个字符得到的序列称为S的子序列。 例如"abcde"的子序列有"abe","","abcde"等。 定义LCS(S,T)为字符串S和字符串T最长公共子序列的长度,即一个最长的序列W既是S的子序列也是T的子序列的长度。 小易给出一个合法的括号匹配序列s,小易希望你能找出具有以下特征的括号序列t: 1、t跟s不同,但是长度相同 2、t也是一个合法的括号匹配序列 3、LCS(s, t)是满足上述两个条件的t中最大的 因为这样的t可能存在多个,小易需要你计算出满足条件的t有多少个。

如样例所示: s = "(())()",跟字符串s长度相同的合法括号匹配序列有: "()(())", "((()))", "()()()", "(()())",其中LCS( "(())()", "()(())" )为4,其他三个都为5,所以输出3. 输入描述: 输入包括字符串s(4 ≤ |s| ≤ 50,|s|表示字符串长度),保证s是一个合法的括号匹配序列。

输出描述：输出一个正整数，满足条件t的个数。

输入例子：(())()

输出例子：3

```c++
#include<iostream>
#include <algorithm>
#include <string>
#include <vector>
using namespace std;

bool check(string s) {
	int count = 0;
	for (int i = 0; i < s.size(); i++) {
		if (s[i] == '(')
			count++;
		if (s[i] == ')')
			count--;
		if (count < 0)
			return false;
	}
	return true;
}
bool isOwn(vector<string> v,string s) {
	for (int i = 0; i < v.size(); i++) {
		if (v[i] == s) {
			return false;
		}
	}
	return true;
}
int main()
{
	string str;
	cin >> str;
	vector<string> vec;
	vec.push_back(str);
	for (int i = 0; i < str.size(); i++) {
		string item = str;
		item.erase(i, 1);
		for (int j = 0; j < item.size(); j++) {
			string tmp = item;
			tmp.insert(j, 1, str[i]);
			if (!check(tmp))
				continue;
			if (!isOwn(vec, tmp))
				continue;
			vec.push_back(tmp);
		}
	}
	cout << vec.size()-1<<endl;
	return 0;
}
```

#### **7、合唱**

题述：小Q和牛博士合唱一首歌曲,这首歌曲由n个音调组成,每个音调由一个正整数表示。 对于每个音调要么由小Q演唱要么由牛博士演唱,对于一系列音调演唱的难度等于所有相邻音调变化幅度之和, 例如一个音调序列是8, 8, 13, 12, 那么它的难度等于|8 - 8| + |13 - 8| + |12 - 13| = 6(其中||表示绝对值)。 现在要对把这n个音调分配给小Q或牛博士,让他们演唱的难度之和最小,请你算算最小的难度和是多少。 如样例所示: 小Q选择演唱{5, 6}难度为1, 牛博士选择演唱{1, 2, 1}难度为2,难度之和为3,这一个是最小难度和的方案了。 输入描述: 输入包括两行,第一行一个正整数n(1 ≤ n ≤ 2000) 第二行n个整数v[i](1 ≤ v[i] ≤ 10^6), 表示每个音调。

输出描述: 输出一个整数,表示小Q和牛博士演唱最小的难度和是多少。

输入例子1: 5 1 5 6 2 1

输出例子1: 3

```c++
#include<iostream>
#include <algorithm>
#include <string>
#include <vector>
using namespace std;

//此题待琢磨自己的方法
int main()
{
	int n;
	cin >> n;
	vector<int> vec;
	for (int i = 0; i < n; i++) {
		int item;
		cin >> item;
		vec.push_back(item);
	}
	vector<vector<int>> dp(n + 1, vector<int>(n + 1, 0));
	for (int i = n - 1; i >= 0; i--) {
		for (int j = n - 1; j >= 0; j--) {
			int p = max(i, j) + 1;
			dp[i][j] = 0x7fffffff;
			dp[i][j] = min(dp[i][j], dp[i][p] + (j == 0 ? 0 : abs(vec[j - 1] - vec[p - 1])));
			dp[i][j] = min(dp[i][j], dp[p][j] + (i == 0 ? 0 : abs(vec[i - 1] - vec[p - 1])));
		}
	}
	cout << dp[0][0];
	return 0;
}
```

#### **8、射击游戏**

题述：小易正在玩一款新出的射击游戏,这个射击游戏在一个二维平面进行,小易在坐标原点(0,0),平面上有n只怪物,每个怪物有所在的坐标(x[i], y[i])。小易进行一次射击会把x轴和y轴上(包含坐标原点)的怪物一次性消灭。 小易是这个游戏的VIP玩家,他拥有两项特权操作: 1、让平面内的所有怪物同时向任意同一方向移动任意同一距离 2、让平面内的所有怪物同时对于小易(0,0)旋转任意同一角度 小易要进行一次射击。小易在进行射击前,可以使用这两项特权操作任意次。 小易想知道在他射击的时候最多可以同时消灭多少只怪物,请你帮帮小易。

如样例所示：

![6910869-1](https://github.com/xingwy/Hugging-Algorithm/blob/master/images/6910869-1.png)

所有点对于坐标原点(0,0)顺时针或者逆时针旋转45°,可以让所有点都在坐标轴上,所以5个怪物都可以消灭。

输入描述: 输入包括三行。 第一行中有一个正整数n(1 ≤ n ≤ 50),表示平面内的怪物数量。 第二行包括n个整数x[i](-1,000,000 ≤ x[i] ≤ 1,000,000),表示每只怪物所在坐标的横坐标,以空格分割。 第二行包括n个整数y[i](-1,000,000 ≤ y[i] ≤ 1,000,000),表示每只怪物所在坐标的纵坐标,以空格分割。

输出描述: 输出一个整数表示小易最多能消灭多少只怪物。

输入例子1：5 0 -1 1 1 -1 0 -1 -1 1 1

输出例子1：5

```c++
#include<iostream>
#include <vector>
using namespace std;

struct point {
	long long pos_x;
	long long pos_y;
	int count = 1;
};

bool judge(point a, point b) {
	if (a.pos_x*b.pos_x + a.pos_y*b.pos_y == 0)
		return true;
	if (a.pos_x*b.pos_y == a.pos_y*b.pos_x)
		return true;
	return false;
}
int main() {
	//同斜率数量最多  但可能会有斜率精度不准的情况，第
	//二种方法，遍历每个点求其的向量的同向或垂直向量
	int n;
	cin >> n;
	vector<point> vec;
	int max = 0;
	for (int i = 0; i < n; i++) {
		point p;
		cin >> p.pos_x;
		vec.push_back(p);
	}
	for (int i = 0; i < n; i++) {
		int y;
		cin >> y;
		vec[i].pos_y = y;
	}
	//遍历每个点求其符合个数
	for (int i = 0; i < n; i++) {
		for (int j = 0; j < n; j++) {
			if (i == j)
				continue;
			if (judge(vec[i], vec[j])) {
				vec[i].count++;
			}
		}
		if (max < vec[i].count)
			max = vec[i].count;
	}
	cout << max << endl;
	return 0;
}
```