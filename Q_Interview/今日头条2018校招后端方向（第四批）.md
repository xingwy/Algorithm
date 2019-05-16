### **今日头条2018校招后端方向（第四批）**

#### **编程题1**

题述：有三只球队，每只球队编号分别为球队1，球队2，球队3，这三只球队一共需要进行 n 场比赛。现在已经踢完了k场比赛，每场比赛不能打平，踢赢一场比赛得一分，输了不得分不减分。已知球队1和球队2的比分相差d1分，球队2和球队3的比分相差d2分，每场比赛可以任意选择两只队伍进行。求如果打完最后的 (n-k) 场比赛，有没有可能三只球队的分数打平。

输入描述：第一行包含一个数字 t (1 <= t <= 10),接下来的t行每行包括四个数字 n, k, d1, d2(1 <= n <= 10^12; 0 <= k <= n, 0 <= d1, d2 <= k)

输出描述：每行的比分数据，最终三只球队若能够打平，则输出“yes”，否则输出“no”

输入示例：2

​		   3 3 0 0 

​         	   3 3 3 3 

输出示例：yes

​		   no

示例说明：

1.球队1和球队2差0分，球队2 和球队3也差0分，所以可能的赛得分是三只球队各得1分

2.球队1和球队2差3分，球队2和球队3差3分，所以可能的得分是球队1得0分，球队2得3分, 球队3得0分，比赛已经全部结束因此最终不能打平。

```c++
#include<iostream>
#include<algorithm>
#include<vector>
using namespace std;

struct T {
	int n;
	int k;
	int d1;
	int d2;
};
bool judge(int n,int k,int d1,int d2) {
	int tmp = k + d2 - d1;
	if (tmp % 3 || tmp < 0) {
		return false;
	}
	int y = tmp / 3;
	int x = d1 + y, z = y - d2;
	if (x < 0 || z < 0) {
		return false;
	}
	vector<int> m;
	m.push_back(x);
	m.push_back(y);
	m.push_back(z);
	sort(m.begin(), m.end());
	if ((m[0] + m[1] + n - k) == 2 * m[2]) {
		return true;
	}
	else {
		return false;
	}
}
int main()      
{
	int t;
	cin >> t;
	vector<T> vec(t);
	for (int i = 0; i < t; i++){
		cin >> vec[i].n >> vec[i].k >> vec[i].d1 >> vec[i].d2;
	}
	for (int i = 0; i < t; i++) {
		bool res = (judge(vec[i].n, vec[i].k, vec[i].d1, vec[i].d2)|| judge(vec[i].n, vec[i].k, vec[i].d1, vec[i].d2)
			|| judge(vec[i].n, vec[i].k, vec[i].d1, vec[i].d2)|| judge(vec[i].n, vec[i].k, vec[i].d1, vec[i].d2));
		if (res) {
			cout << "yes" << endl;
		}
		else {
			cout << "no" << endl;
		}
	}
	return 0;
}
```

#### **编程题2**

题述：有一个仅包含’a’和’b’两种字符的字符串s，长度为n，每次操作可以把一个字符做一次转换（把一个’a’设置为’b’，或者把一个’b’置成’a’)；但是操作的次数有上限m，问在有限的操作数范围内，能够得到最大连续的相同字符的子串的长度是多少。 

输入描述：第一行两个整数 n , m (1<=m<=n<=50000)，第二行为长度为n且只包含’a’和’b’的字符串s。

输出描述：输出在操作次数不超过 m 的情况下，能够得到的 最大连续 全’a’子串或全’b’子串的长度。

输入示例：8 1

​		   aabaabaa

输出示例：5

示例说明：把第一个 'b' 或者第二个 'b' 置成 'a'，可得到长度为 5 的全 'a' 子串。

```c++
#include<iostream>
#include<algorithm>
#include<vector>
using namespace std;

int main()      
{
	int n, m;
	cin >> n >> m;
	string str;
	cin >> str;
    //建立两个表 分别记录a,b在字符串出现的相对下标  比如字符串为aabaabaa
    //vec_a[1,2,4,5,7,8] vec_b[3,6]
	vector<int> vec_a(1, 1), vec_b(1, 1);
	for (int i = 0; i < n; i++) {
		if (str[i] == 'a') {
			vec_a.push_back(i+1);
		}
		else if (str[i] == 'b') {
			vec_b.push_back(i+1);
		}
	}
	vec_a.push_back(n);
	vec_b.push_back(n);
	int maxRes = 0;
	for (int i = 1 + m; i < vec_a.size(); i++) {
		maxRes = max(maxRes, vec_a[i] - vec_a[i - m - 1]);
	}
	for (int i = 1 + m; i < vec_b.size(); i++) {
		maxRes = max(maxRes, vec_b[i] - vec_b[i - m - 1]);
	}
	cout << maxRes;
	return 0;
}
```

#### **附加题**

题述：存在n+1个房间，每个房间依次为房间1 2 3...i，每个房间都存在一个传送门，i房间的传送门可以把人传送到房间pi(1<=pi<=i),现在路人甲从房间1开始出发(当前房间1即第一次访问)，每次移动他有两种移动策略：

A. 如果访问过当前房间 i 偶数次，那么下一次移动到房间i+1；

B. 如果访问过当前房间 i 奇数次，那么移动到房间pi；

现在路人甲想知道移动到房间n+1一共需要多少次移动；

输入描述：第一行包括一个数字n(30%数据1<=n<=100，100%数据 1<=n<=1000)，表示房间的数量，接下来一行存在n个数字 pi(1<=pi<=i), pi表示从房间i可以传送到房间pi。

输出描述：输出一行数字，表示最终移动的次数，最终结果需要对1000000007 (10e9 + 7) 取模。

输入示例：2

 		   1 2

输出示例：4

示例说明：开始从房间1只访问一次所以只能跳到p1即房间1，之后采用策略A跳到房间2，房间2这时访问了一次因此采用策略B跳到房间2，之后采用策略A跳到房间3，因此到达房间3需要4步操作。

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
	//dp[i]表示从起点到点i的步数
	vector<long long> dp(n + 1, 0);
	for (int i = 0; i < n; i++) {
		//dp[i+1] = dp[i]+1 +(dp[i]-dp[vec[i]-1]+1)
		dp[i + 1] = 2 * dp[i] + 1 - dp[vec[i] - 1] + 1;
		dp[i + 1] = (dp[i + 1] + 1000000007) % 1000000007;
	}
	cout << dp[n];
	return 0;
}
```

