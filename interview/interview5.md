### **爱奇艺2018秋招算法工程师（第一场）**

#### **1、括号匹配深度**

题述：一个合法的括号匹配序列有以下定义: 

1. 空串""是一个合法的括号匹配序列 

2. 如果"X"和"Y"都是合法的括号匹配序列,"XY"也是一个合法的括号匹配序列 

3. 如果"X"是一个合法的括号匹配序列,那么"(X)"也是一个合法的括号匹配序列 

4. 每个合法的括号序列都可以由以上规则生成。 

   例如: "","()","()()","((()))"都是合法的括号序列 对于一个合法的括号序列我们又有以下定义它的深度:

    1、空串""的深度是0 

    2、如果字符串"X"的深度是x,字符串"Y"的深度是y,那么字符串"XY"的深度为max(x,y) 

    3、如果"X"的深度是x,那么字符串"(X)"的深度是x+1 例如: "()()()"的深度是1,"((()))"的深度是3。

    牛牛现在给你一个合法的括号序列,需要你计算出其深度。

输入描述：输入包括一个合法的括号序列s,s长度length(2 ≤ length ≤ 50),序列中只包含'('和')'。

输出描述：输出一个正整数,即这个序列的深度。

输入示例：(())

输出示例：2

```c++
#include<iostream>
#include<algorithm>
#include<string>
using namespace std;

int main()      
{
	string str;
	cin >> str;
	int depth = 0, max_depth = 0;
	for (int i = 0; i < str.size(); i++) {
		if (str[i] == '(') {
			depth++;
		}
		else if (str[i] == ')') {
			depth--;
		}
		max_depth = max(depth, max_depth);
	}
	cout << max_depth;
	return 0;
}
```

#### **2、奶牛编号**

题述：牛牛养了n只奶牛,牛牛想给每只奶牛编号,这样就可以轻而易举地分辨它们了。 每个奶牛对于数字都有自己的喜好,第i只奶牛想要一个1和x[i]之间的整数(其中包含1和x[i])。 牛牛需要满足所有奶牛的喜好,请帮助牛牛计算牛牛有多少种给奶牛编号的方法,输出符合要求的编号方法总数。

输入描述：输入包括两行,第一行一个整数n(1 ≤ n ≤ 50),表示奶牛的数量 第二行为n个整数x[i](1 ≤ x[i] ≤ 1000)

输出描述：输出一个整数,表示牛牛在满足所有奶牛的喜好上编号的方法数。因为答案可能很大,输出方法数对1,000,000,007的模。

输入示例：4 

​		   4 4 4 4

输出示例：24

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
	int res = 1;
	sort(vec.begin(), vec.end());
	for (int i = 0; i < n; i++) {
		res = ((res % 1000000007)*(vec[i] - i)) % 1000000007;
	}
	cout << res;
	return 0;
}
```

#### **3、平方串**

题述：如果一个字符串S是由两个字符串T连接而成,即S = T + T, 我们就称S叫做平方串,例如"","aabaab","xxxx"都是平方串. 牛牛现在有一个字符串s,请你帮助牛牛从s中移除尽量少的字符,让剩下的字符串是一个平方串。换句话说,就是找出s的最长子序列并且这个子序列构成一个平方串。

输入描述：输入一个字符串s,字符串长度length(1 ≤ length ≤ 50),字符串只包括小写字符。

输出描述：输出一个正整数,即满足要求的平方串的长度。

输入示例：frankfurt

输出示例：4

```c++
#include<iostream>
#include<algorithm>
#include<string>
using namespace std;

int getMaxStrs(string str1,string str2) {
	vector<vector<int>> dp(str1.size() + 1, vector<int>(str2.size() + 1, 0));
	for (int i = 1; i <= str1.size(); i++) {
		for (int j = 1; j <= str2.size(); j++) {
			if (str1[i] == str2[j]) {
				dp[i][j] = dp[i - 1][j - 1] + 1;
			}
			else {
				dp[i][j] = max(dp[i - 1][j], dp[i][j - 1]);
			}
		}
	}
	return dp[str1.size()][str2.size()];
}
int main()      
{
	
	string str;
	cin >> str;
	int res = 0;
	//将一个字符串分割成两个子串，然后演化成求两个子串的最大公共子串问题。
    //最后找出每一种情况的最大值
	for (int i = 1; i < str.size(); i++) {
		string str1(str.substr(0,i)), str2(str.substr(i,str.size()));
		res = max(res, getMaxStrs(str1, str2));
	}
	cout << 2 * res;
	return 0;
}
```

