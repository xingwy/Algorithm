### **网易2017秋招编程题**

#### **1、回文序列**

题述：如果一个数字序列逆置之后跟原序列是一样的就称这样的数字序列为回文序列。例如： {1, 2, 1}, {15, 78, 78, 15} , {112} 是回文序列, {1, 2, 2}, {15, 78, 87, 51} ,{112, 2, 11} 不是回文序列。 现在给出一个数字序列，允许使用一种转换操作： 选择任意两个相邻的数，然后从序列移除这两个数，并用这两个数字的和插入到这两个数之前的位置(只插入一个和)。 现在对于所给序列要求出最少需要多少次操作可以将其变成回文序列。

输入描述：输入为两行，第一行为序列长度n ( 1 ≤ n ≤ 50) 第二行为序列中的n个整数item[i] (1 ≤ iteam[i] ≤ 1000)，以空格分隔。

输出描述：输出一个数，表示最少需要的转换次数

输入示例：4 

​	   	   1 1 1 3

输出示例：2

```c++
#include<iostream>
#include <algorithm>
#include <vector>
using namespace std;

int main()      
{
	int n;
	cin >> n;
	vector<int> vec;
	for (int i = 0; i < n; i++) {
		int tmp;
		cin >> tmp;
		vec.push_back(tmp);
	}
	int start = 0, end = n - 1;
	int count = 0;
	while (start <= end) {
		if (vec[start] < vec[end]) {
			vec[start + 1] += vec[start];
			start++;
			count++;
		}
		else if (vec[start] > vec[end]) {
			vec[end - 1] += vec[end];
			end--;
			count++;
		}
		else {
			start++;
			end--;
		}
	}
	cout << count;
	return 0;
}
```

#### **2、优雅的点**

题述：小易有一个圆心在坐标原点的圆，小易知道圆的半径的平方。小易认为在圆上的点而且横纵坐标都是整数的点是优雅的，小易现在想寻找一个算法计算出优雅的点的个数，请你来帮帮他。 例如：半径的平方如果为25 优雅的点就有：(+/-3, +/-4), (+/-4, +/-3), (0, +/-5) (+/-5, 0)，一共12个点。

输入描述：输入为一个整数，即为圆半径的平方,范围在32位int范围内。

输出描述：输出为一个整数，即为优雅的点的个数 

输入示例：25

输出示例：12

```c++
#include<iostream>
#include <algorithm>
using namespace std;

bool judge(int n, int i) {
	int item = n - i * i;
	int tmp = int(sqrt(item));
	return tmp * tmp == item;
}
int main()      
{
	int n;
	cin >> n;
	int count = 0;
	if (judge(n, 0)) {
		count += 4;
	}
	if (judge(n / 2, 0) && n%2 == 0) {
		count += 4;
	}
	for (int i = 1; i < sqrt(n / 2); i++) {
		if (judge(n, i)) {
			count += 8;
		}
	}
	cout << count;
	return 0;
}
```

#### **3、跳石板**

题述：小易来到了一条石板路前，每块石板上从1挨着编号为：1、2、3....... 这条石板路要根据特殊的规则才能前进：对于小易当前所在的编号为K的 石板，小易单次只能往前跳K的一个约数(不含1和K)步，即跳到K+X(X为K的一个非1和本身的约数)的位置。 小易当前处在编号为N的石板，他想跳到编号恰好为M的石板去，小易想知道最少需要跳跃几次可以到达。 例如： N = 4，M = 24： 4->6->8->12->18->24 于是小易最少需要跳跃5次，就可以从4号石板跳到24号石板。

输入描述：输入为一行，有两个整数N，M，以空格隔开。 (4 ≤ N ≤ 100000) (N ≤ M ≤ 100000)

输出描述：输出小易最少需要跳跃的步数,如果不能到达输出-1 

输入示例：4 24

输出示例：5

```c++
#include<iostream>
#include <algorithm>
#include <vector>
using namespace std;

vector<int> res(100005, 0x7fffff);
void dp(int tmp) {
	vector<int> vec;
	for (int i = 2; i <= tmp - 1; i++) {
		if (tmp%i == 0)
			vec.push_back(i);
	}
	for (int i = 0; i < vec.size(); i++) {
		res[tmp + vec[i]] = min(res[tmp] + 1, res[tmp + vec[i]]);
	}
}

int main()      
{
	int s, e;
	cin >> s >> e;
	res[s] = 0;
	for (int i = s; i <= e; i++) {
		dp(i);
	}
	if (res[e] >= 0x7fffff) {
		cout << -1;
	}
	else {
		cout << res[e];
	}
	return 0;
}
```

#### **4、暗黑的字符串**

题述：一个只包含'A'、'B'和'C'的字符串，如果存在某一段长度为3的连续子串中恰好'A'、'B'和'C'各有一个，那么这个字符串就是纯净的，否则这个字符串就是暗黑的。例如： BAACAACCBAAA 连续子串"CBA"中包含了'A','B','C'各一个，所以是纯净的字符串 AABBCCAABB 不存在一个长度为3的连续子串包含'A','B','C',所以是暗黑的字符串 你的任务就是计算出长度为n的字符串(只包含'A'、'B'和'C')，有多少个是暗黑的字符串。

输入描述：输入一个整数n，表示字符串长度(1 ≤ n ≤ 30)

输出描述：输出一个整数表示有多少个暗黑字符串

输入示例：2 3 

输出示例：9 21

```c++
#include<iostream>
#include <algorithm>
#include <vector>
using namespace std;

int main()      
{
	int n;
	cin >> n;
	vector<long long> dp(n+1,0);
	dp[1] = 3;  //3的一次方
	dp[2] = 9;  //3的平方
	//dp[i]代表长度为i时的暗黑字符串个数
	for (int i = 3; i <= n; i++) {
		dp[i] = 2 * dp[i - 1] + dp[i - 2];
	}
	cout << dp[n];
	return 0;
}
```

#### **5、数字翻转**

题述：对于一个整数X，定义操作rev(X)为将X按数位翻转过来，并且去除掉前导0。例如: 如果 X = 123，则rev(X) = 321; 如果 X = 100，则rev(X) = 1. 现在给出整数x和y,要求rev(rev(x) + rev(y))为多少？

输入描述：输入为一行，x、y(1 ≤ x、y ≤ 1000)，以空格隔开。

输出描述：输出rev(rev(x) + rev(y))的值

输入示例：123 100

输出示例：223

```c++
#include<iostream>
#include <algorithm>
#include <sstream>
#include <string>
using namespace std;

int rev(int i) {
	string  str;
	int tmp;
	stringstream stream_itos;
	stream_itos << i;
	str = stream_itos.str();
	reverse(str.begin(), str.end());
	stringstream stream_stoi(str);
	stream_stoi >> tmp;
	return tmp;
}
int main()      
{
	int x, y;
	cin >> x >> y;
	int tmp = rev(rev(x) + rev(y));
	cout << tmp;
	return 0;
}
```

#### **6、最大的奇约数**

题述：小易是一个数论爱好者，并且对于一个数的奇数约数十分感兴趣。一天小易遇到这样一个问题： 定义函数f(x)为x最大的奇数约数，x为正整数。 例如:f(44) = 11. 现在给出一个N，需要求出 f(1) + f(2) + f(3).......f(N) 例如： N = 7 f(1) + f(2) + f(3) + f(4) + f(5) + f(6) + f(7) = 1 + 1 + 3 + 1 + 5 + 3 + 7 = 21 小易计算这个问题遇到了困难，需要你来设计一个算法帮助他。

输入描述：输入一个整数N (1 ≤ N ≤ 1000000000)

输出描述：输出一个整数，即为f(1) + f(2) + f(3).......f(N)

输入示例：7

输出示例：21

```c++
#include<iostream>
#include <algorithm>
using namespace std;

long long dps(long long n) {
	/*此题数据n上限1000000000 一个一个求其最大奇约数明显不理智
	//所以笔者在初始就在想肯定有简便的方法。后来看到一个牛客的
	//概念示例才豁然开朗。
	//思路在于对于[1,n]的奇数，最大奇约数取其自身，对于区间的
	//偶数最大奇约数和，其值就等于[1,n/2]的所有奇约数和。
	*/
	if (n == 1)
		return 1;
	if (n & 1) {
		//奇数        
		return (n + 1)*(n + 1) / 4 + dps(n >> 1);
	}
	else {
		//偶数
		return n * n / 4 + dps(n >> 1);
	}
}
int main()      
{
	long long n, sum;
	cin >> n;
	sum = dps(n);
	cout << sum;
	return 0;
}
```

#### **7、买苹果**

题述：小易去附近的商店买苹果，奸诈的商贩使用了捆绑交易，只提供6个每袋和8个每袋的包装(包装不可拆分)。 可是小易现在只想购买恰好n个苹果，小易想购买尽量少的袋数方便携带。如果不能购买恰好n个苹果，小易将不会购买。 

输入描述：输入一个整数n，表示小易想购买n(1 ≤ n ≤ 100)个苹果。

输出描述：输出一个整数表示最少需要购买的袋数，如果不能买恰好n个苹果则输出-1。

输入示例：20 

输出示例：3

```c++
#include<iostream>
#include <algorithm>
using namespace std;

int main()      
{
	int n;
	cin >> n;
	int x = n / 8, y = 0;
	int count = -1;
	for (int i = x; i >= 0; i--) {
		while (true) {
			if ((8 * i + y * 6) >= n)
				break;
			y++;
		}
		if ((8 * i + y * 6) == n) {
			count = y + i;
			break;
		}
	}
	cout << count;
	return 0;
}
```

#### **8、计算糖果**

题述：A,B,C三个人是好朋友,每个人手里都有一些糖果,我们不知道他们每个人手上具体有多少个糖果,但是我们知道以下的信息： A - B, B - C, A + B, B + C. 这四个数值.每个字母代表每个人所拥有的糖果数. 现在需要通过这四个数值计算出每个人手里有多少个糖果,即A,B,C。这里保证最多只有一组整数A,B,C满足所有题设条件。

输入描述：输入为一行，一共4个整数，分别为A - B，B - C，A + B，B + C，用空格隔开。 范围均在-30到30之间(闭区间)。

输出描述： 输出为一行，如果存在满足的整数A，B，C则按顺序输出A，B，C，用空格隔开，行末无空格。 如果不存在这样的整数A，B，C，则输出No

输入示例：1 -2 3 4

输出示例：2 1 3

```c++
#include<iostream>
using namespace std;

int main()      
{
	int d1, d2, d3, d4;
	cin >> d1 >> d2 >> d3 >> d4;
	int A, B, C;
	A = (d1 + d3) / 2;
	B = (d2 + d4) / 2;
	C = (d4 - d2) / 2;
	if (A < 0 || B < 0 || C < 0 || A - B != d1 || B - C != d2 || A + B != d3 || B + C != d4) {
		cout << "NO";
	}
	else {
		cout << A << " " << B << " " << C;
	}
	return 0;
}
```

