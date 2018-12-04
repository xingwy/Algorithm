### **爱奇艺2018秋招算法工程师（第二场）**

#### **1、青草游戏**

题述：牛牛和羊羊都很喜欢青草。今天他们决定玩青草游戏。 最初有一个装有n份青草的箱子,牛牛和羊羊依次进行,牛牛先开始。在每个回合中,每个玩家必须吃一些箱子中的青草,所吃的青草份数必须是4的x次幂,比如1,4,16,64等等。不能在箱子中吃到有效份数青草的玩家落败。假定牛牛和羊羊都是按照最佳方法进行游戏,请输出胜利者的名字。

输入描述：输入包括t+1行。 第一行包括一个整数t(1 ≤ t ≤ 100),表示情况数. 接下来t行每行一个n(1 ≤ n ≤ 10^9),表示青草份数

输出描述：对于每一个n,如果牛牛胜利输出"niu",如果羊羊胜利输出"yang"。

输入示例：3 

​		   1 2 3 

输出示例：niu yang niu

```c++
#include<iostream>
using namespace std;

int main()      
{
	int n;
	cin >> n;
	for (int i = 0; i < n; i++) {
		int tmp;
		cin >> tmp;
		if (tmp % 5 == 0 || tmp % 5 == 2) {
			cout << "yang" << endl;
		}
		else {
			cout << "niu" << endl;
		}
	}
	return 0;
}
```

#### **2、无聊的牛牛和羊羊**

题述：牛牛和羊羊非常无聊.他们有n + m个共同朋友,他们中有n个是无聊的,m个是不无聊的。每个小时牛牛和羊羊随机选择两个不同的朋友A和B.(如果存在多种可能的pair(A, B),任意一个被选到的概率相同。),然后牛牛会和朋友A进行交谈,羊羊会和朋友B进行交谈。在交谈之后,如果被选择的朋友之前不是无聊会变得无聊。现在你需要计算让所有朋友变得无聊所需要的时间的期望值。

输入描述：输入包括两个整数n 和 m(1 ≤ n, m ≤ 50)

输出描述：输出一个实数,表示需要时间的期望值,四舍五入保留一位小数。

输入示例：2 1

输出示例：1.5

```c++
#include <iostream>
#include <cmath>
using namespace std;  // 该代码为示例代码

inline double C2(int n)
{
	return n*(n - 1) / 2.0;
}
int main()
{
	int n, m;
	cin >> n >> m;
	//当前选择有三种情况
	//1 n选2 m选0
	//2 n选1 m选1
	//3 n选0 m选2
	//分别计算三种情况的概率，然后递推 f(k) = 1 + f(k)*p(3)+f(k-1)*p(1)+f(k-2)*p(2) => f(k) = [1+f(k-1)*p(1)+f(k-2)*p(2)]/[1-p(3)]
	int all = n + m;
	double div = all*(all - 1) / 2.0;//c(all,2)
	double f0 = 0, f1 = all / 2.0;
	for (int i = 2; i <= m; i++)
	{
		//还剩i个不无聊人时的概率
		//无聊人=all-i
		double tmp = (1 + C2(i) / div*f0 + i*(all - i) / div*f1) / (1 - C2(all - i) / div);
		f0 = f1;
		f1 = tmp;
	}
	printf("%.1lf\n", round(f1 * 10) / 10);
	return 0;
}
```

#### **3、幸运子序列**

题述：牛牛得到一个长度为n的整数序列V,牛牛定义一段连续子序列的幸运值为这段子序列中最大值和次大值的异或值(次大值是严格的次大)。牛牛现在需要求出序列V的所有连续子序列中幸运值最大是多少。请你帮帮牛牛吧。

输入描述：第一行一个整数n,即序列的长度。(2<= n <= 100000) 第二行n个数，依次表示这个序列每个数值V[i], (1 ≤ V[i] ≤ 10^8)且保证V[1]到V[n]中至少存在不同的两个值.

输出描述：输出一个整数,即最大的幸运值 

输入示例：5 

​		   5 2 1 4 3 

输出示例：7

```c++
#include<iostream>
#include<algorithm>
#include<vector>
using namespace std;

int main()      
{
	int n;
	cin >> n;
	vector<int> vec(n);
	for (int i = 0; i < n; i++) {
		cin >> vec[i];
	}
	int res = 0;
	for (int i = 0; i < n; i++) {
		int fst_max = vec[i], sec_max = 0;
		int l = i - 1, r = i + 1;
		while (l >= 0) {
			if (vec[l] >= fst_max)
				break;
			sec_max = max(sec_max, vec[l]);
			res = max(res, fst_max^sec_max);
			l--;
		}
		sec_max = 0;
		while (r < n) {
			if (vec[r] >= fst_max)
				break;
			sec_max = max(sec_max, vec[r]);
			res = max(res, fst_max^sec_max);
			r++;
		}
	}
	cout << res;
	return 0;
}
```

