### **今日头条2018校招后端方向（第二批）**

#### **1、用户喜好**

题述：为了不断优化推荐效果，今日头条每天要存储和处理海量数据。假设有这样一种场景：我们对用户按照它们的注册时间先后来标号，对于一类文章，每个用户都有不同的喜好值，我们会想知道某一段时间内注册的用户（标号相连的一批用户）中，有多少用户对这类文章喜好值为k。因为一些特殊的原因，不会出现一个查询的用户区间 完全覆盖另一个查询的用户区间(不存在L1<=L2<=R2<=R1)。

输入描述：第1行为n代表用户的个数 第2行为n个整数，第i个代表用户标号为i的用户对某类文章的喜好度 第3行为一个正整数q代表查询的组数  第4行到第（3+q）行，每行包含3个整数l,r,k代表一组查询，即标号为l<=i<=r的用户中对这类文章喜好值为k的用户的个数。 数据范围n <= 300000,q<=300000，k是整型。

输出描述：一共q行，每行一个整数代表喜好值为k的用户的个数

输入示例：5

​		   1 2 3 3 5

​		   3

​                   1 2 1

​		   2 4 5

​		   3 5 3

输出示例：1

​		   0

​		   2

```c++
#include<iostream>
#include<algorithm>
#include<vector>
#include<map>
#include<set>
using namespace std;

int main()      
{
	int n;
	cin >> n;
	map<int, vector<int>> dp;
	vector<int> res;
    //将相同喜好的人储存在一起
	for (int i = 1; i <= n; i++) {
		int tmp;
		cin >> tmp;
		dp[tmp].push_back(i);
	}
	int q;
	cin >> q;
	for (int i = 0; i < q; i++) {
		int l, r, k;
		cin >> l >> r >> k;
		int cnt = 0;
		for (int j = 0; j < dp[k].size(); j++) {
			if (dp[k][j] < l) {
				continue;
			}
			if (dp[k][j] > r) {
				break;
			}
			cnt++;
		}
		res.push_back(cnt);
	}
	for (int i = 0; i < res.size(); i++) {
		cout << res[i] << endl;
	}
	return 0;
}
```

#### **2、手串**

题述：作为一个手串艺人，有金主向你订购了一条包含n个杂色串珠的手串——每个串珠要么无色，要么涂了若干种颜色。为了使手串的色彩看起来不那么单调，金主要求，手串上的任意一种颜色（不包含无色），在任意连续的m个串珠里至多出现一次（注意这里手串是一个环形）。手串上的颜色一共有c种。现在按顺时针序告诉你n个串珠的手串上，每个串珠用所包含的颜色分别有哪些。请你判断该手串上有多少种颜色不符合要求。即询问有多少种颜色在任意连续m个串珠中出现了至少两次。

输入描述：第一行输入n，m，c三个数，用空格隔开。(1 <= n <= 10000, 1 <= m <= 1000, 1 <= c <= 50) 接下来n行每行的第一个数num_i(0 <= num_i <= c)表示第i颗珠子有多少种颜色。接下来依次读入num_i个数字，每个数字x表示第i颗珠子上包含第x种颜色(1 <= x <= c)

输出描述：一个非负整数，表示该手链上有多少种颜色不符需求。

输入示例：5 2 3

​		   3 1 2 3

  		   0

 		   2 2 3

​		   1 2

  		   1 3

输出示例：2

示例说明：

```
第一种颜色出现在第1颗串珠，与规则无冲突。
第二种颜色分别出现在第 1，3，4颗串珠，第3颗与第4颗串珠相邻，所以不合要求。
第三种颜色分别出现在第1，3，5颗串珠，第5颗串珠的下一个是第1颗，所以不合要求。
总计有2种颜色的分布是有问题的。 
这里第2颗串珠是透明的。
```

```c++
#include<iostream>
#include<algorithm>
#include<vector>
using namespace std;

int n, m, c;
bool judge(vector<int>& t) {
	if (t.size() == 0) {
		return true;
	}
	for (int i = 0; i < t.size() - 1; i++) {
		if ((t[i + 1] - t[i]) < m) {
			return false;
		}
	}
	if ((t[0] + n - t[t.size() - 1]) < m) {
		return false;
	}
	return true;
}
int main()      
{
	cin >> n >> m >> c;
	vector<vector<int>> vec(c + 1);
	for (int i = 0; i < n; i++) {
		int num, x;
		cin >> num;
		for (int j = 0; j < num; j++) {
			//x表示颜色  i表示珠子下标
			cin >> x;
			vec[x].push_back(i + 1);
		}
	}
	int res = 0;
	for (int i = 1; i <= c; i++) {
		res += (!judge(vec[i]) ? 1 : 0);
	}
	cout << res;
	return 0;
}
```

#### **3、字母交换**

题述：字符串S由小写字母构成，长度为n。定义一种操作，每次都可以挑选字符串中任意的两个相邻字母进行交换。询问在至多交换m次之后，字符串中最多有多少个连续的位置上的字母相同？

输入描述：第一行为一个字符串S与一个非负整数m。(1 <= |S| <= 1000, 1 <= m <= 1000000)

输出描述：一个非负整数，表示操作之后，连续最长的相同字母数量。

输入示例：abcbaa 2

输出示例：2

示例说明：使3个字母a连续出现，至少需要3次操作。即把第1个位置上的a移动到第4个位置。所以在至多操作2次的情况下，最多只能使2个b或2个a连续出现。

```c++
#include<iostream>
#include<algorithm>
#include<string>
#include<vector>
using namespace std;

string str;
int M;
int getMaxCnt(int cur) {
	int l = cur, r = cur;
	int m = M;
	//先找出左边和右边的首个相同字符
	int pos_i = 1;
	while (true) {
		//左边存在 且 左边近
		if ((l - pos_i) < 0 && (r + pos_i) >= str.size()) {
			break;
		}
		else if (l - pos_i < 0 && r + pos_i < str.size()) {
			//左边超长  取右边
			if (str[cur] == str[r + pos_i]) {
				m -= (pos_i - 1);
				if (m < 0) {
					break;
				}
				r++;
			}
			else {
				pos_i++;
			}
			
		}
		else if (l - pos_i >= 0 && r + pos_i >= str.size()) {
			//右边超长  取左边
			if (str[cur] == str[l - pos_i]) {
				m -= (pos_i - 1);
				if (m < 0) {
					break;
				}
				l--;
			}
			else {
				pos_i++;
			}
		}
		else {
			//左右都没超长
			if (str[cur] == str[l - pos_i]) {
				m -= (pos_i - 1);
				if (m < 0) {
					break;
				}
				l--;
			}
			else if (str[cur] == str[r + pos_i]) {
				m -= (pos_i - 1);
				if (m < 0) {
					break;
				}
				r++;
			}
			else {
				pos_i++;
			}
		}
	}
	return r - l + 1;
}
int main()
{
	//以每一个字符作为不动字符 左右展开选取需要交换的字符  遍历数组
	cin >> str >> M;
	int maxres = 0;
	for (int i = 0; i < str.size(); i++) {
		maxres = max(maxres, getMaxCnt(i));
	}
	cout << maxres;
	return 0;
}
```





