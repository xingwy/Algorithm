### **全国2018校招统一模拟题**

#### **1、密码翻译**

题述：在情报传递过程中，为了防止情报被截获，往往需要对情报用一定的方式加密，简单的加密算法虽然不足以完全避免情报被破译，但仍然能防止情报被轻易的识别。我们给出一种最简的的加密方法，对给定的一个字符串，把其中从a-y,A-Y的字母用其后继字母替代，把z和Z用a和A替代，则可得到一个简单的加密字符串。

输入描述：用gets(s)方式读取这一行字符串.每个字符串长度小于80个字符。

输出描述：对于每组数据，输出每行字符串的加密字符串。

输入示例：Hello! How are you!

输出示例：Ifmmp! Ipx bsf zpv!

```c++
#include<iostream>
#include<string>
using namespace std;

bool judge(char a) {
	return (a <= 'Z' && a >= 'A') || (a <= 'z'&&a >= 'a');
}
int main()      
{
	string str;
	getline(cin,str);
	for (int i = 0; i < str.size(); i++) {
		if (judge(str[i])) {
			str[i] += 1;
		}
	}
	cout << str;
	return 0;
}
```

#### **2、寻宝**

题述：亮亮解出了卷轴隐藏的秘密，来到了一片沼泽地。这里有很多空地，而面试直通卡可能埋在任意一块空地中，好在亮亮发现了一堆木材，他可以将木材铺在两个空地之间的沼泽地上。因为亮亮不知道面试直通卡具体在哪一块空地中，所以必须要保证任意一块空地对于亮亮来说是可以抵达的。 “怎么还有鳄鱼！没办法，看来有些空地不能直接到达了。” 亮亮虽然没有洁癖，但是沼泽地实在太臭了，所以亮亮不会循环利用木材。而且木材不能拼接在一起使用，所以亮亮必须要知道在耗费木材最少的情况下，最长的那根木材至少需要多长。

输入描述：第一行包含两个整数N(1≤N≤10000),M(1≤M≤1000000)。N表示公有N块空地。 接下来M行，每行包含三个整数P(1≤P≤N),Q(1≤Q≤N),K代表P,Q两个间没有鳄鱼，需要耗费K的木材。

输出描述：一个整数，即耗费木材最少的情况下，最长的那根木材长度。

输入示例：4 3 

​		   1 2 1 

​		   2 3 1 

​		   3 4 2

输出示例：2

```c++
#include<iostream>
#include<algorithm>
#include<vector>
using namespace std;

struct edge {
	int len;
	int pos_a;
	int pos_b;
};
bool compare(edge a, edge b) {
	return a.len > b.len;
}
int main()      
{
	int n, m;
	cin >> n >> m;
	vector<int> vec_n(n+1, 0);
	vector<edge> vec_e(m);
	for (int i = 0; i < m; i++) {
		int p, q, k;
		cin >> p >> q >> k;
		vec_e[i].pos_a = p;
		vec_e[i].pos_b = q;
		vec_e[i].len = k;
		vec_n[p]++;
		vec_n[q]++;
	}
	sort(vec_e.begin(), vec_e.end(), compare);
	//最短图路径和，按边的长度降序排列，每次去最长的边（前提是边两点有的其他边）
	for (int i = 0; i < vec_e.size(); i++) {
		if (vec_n[vec_e[i].pos_a] == 1 || vec_n[vec_e[i].pos_b] == 1) {
			continue;
		}
		else {
			vec_n[vec_e[i].pos_a] -= 1; 
			vec_n[vec_e[i].pos_b] -= 1;
			vec_e[i].len = -1;
		}
	}
	for (int i = 0; i < vec_e.size(); i++) {
		if (vec_e[i].len != -1) {
			cout << vec_e[i].len;
			break;
		}
	}
	return 0;
}
```

#### **3、打车**

题述：妞妞参加完Google Girl Hackathon之后,打车回到了牛家庄。妞妞需要支付给出租车司机车费s元。妞妞身上一共有n个硬币，第i个硬币价值为p[i]元。妞妞想选择尽量多的硬币，使其总价值足以支付s元车费(即大于等于s)。但是如果从妞妞支付的这些硬币中移除一个或者多个硬币，剩下的硬币总价值还是足以支付车费的话，出租车司机是不会接受的。例如: 妞妞使用价值为2，5，7的硬币去支付s=11的车费,出租车司机是不会接受的，因为价值为2这个硬币是可以移除的。妞妞希望能选取最大数量的硬币，使其总价值足以支付车费并且出租车司机能接受。妞妞希望你能帮她计算最多可以支付多少个硬币。

输入描述：输入包括两行, 第一行包括两个正整数n和s(1 <= n <= 10, 1 <= s <= 1000), 表示妞妞的硬币个数和需要支付的车费。 第二行包括n个正整数p[i] (1 <= p[i] <= 100)，表示第i个硬币的价值。 保证妞妞的n个硬币价值总和是大于等于s。

输出描述：输出一个整数, 表示妞妞最多可以支付的硬币个数。

输入示例：5 9 

​		   4 1 3 5 4

输出示例：3

```c++
#include<iostream>
#include<algorithm>
#include<vector>
using namespace std;

vector<int> coin;
int dfs(int base) {

	if (base <= 0) {
		return 0;
	}
	int pay = 0, pos;
	for (int i = 0; i < coin.size(); i++) {
		pay += coin[i];
		if (pay >= base) {
			pos = i;
			break;
		}
	}
	return dfs(base - coin[pos]) + 1;
}
int main()      
{
	int n, s;
	cin >> n >> s;
	
	for (int i = 0; i < n; i++) {
		int tmp;
		cin >> tmp;
		coin.push_back(tmp);
	}
	sort(coin.begin(), coin.end());
	cout << dfs(s);
	return 0;
}
```

#### **4、美丽的项链**

题述：妞妞参加了Nowcoder Girl女生编程挑战赛, 但是很遗憾, 她没能得到她最喜欢的黑天鹅水晶项链。

于是妞妞决定自己来制作一条美丽的项链。一条美丽的项链需要满足以下条件:

1、需要使用n种特定的水晶宝珠

2、第i种水晶宝珠的数量不能少于li颗, 也不能多于ri颗

3、一条美丽的项链由m颗宝珠组成

妞妞意识到满足条件的项链种数可能会很多, 所以希望你来帮助她计算一共有多少种制作美丽的项链的方案。

输入描述：输入包括n+1行, 第一行包括两个正整数(1 <= n <= 20, 1 <= m <= 100), 表示水晶宝珠的种数和一条美丽的项链需要的水晶宝珠的数量。接下来的n行, 每行两个整数li, ri(0 <= li <= ri <= 10), 表示第i种宝珠的数量限制区间。

输出描述：输出一个整数, 表示满足限定条件的方案数。保证答案在64位整数范围内。

输入示例：3 5 

​		   0 3 

​                   0 3 

​		   0 3

输出示例：12

```c++
#include<iostream>
#include<algorithm>
#include<vector>
using namespace std;

struct T {
	int l;
	int r;
};
int main()      
{
	int n, m;
	cin >> n >> m;
	vector<T> vec(n + 1);
	for (int i = 1; i <= n; i++) {
		T t;
		cin >> t.l >> t.r;
		vec[i] = t;
	}
	//dp[i][j]代表用i种珠子组成j长度的手串方法
	long long dp[25][105] = { {0} };
	//只有第一种珠子
	for (int i = vec[1].l; i <= vec[1].r; i++) {
		dp[1][i] = 1;
	}
	int start = vec[1].l, end = vec[1].r;
	for (int i = 2; i <= n; i++) {
		start += vec[i].l;
		end += vec[i].r;
		//组成的长度范围区间[start,min(m,end)]
		for (int j = start; j <= min(m, end); j++) {
			for (int k = vec[i].l; k <= vec[i].r; k++) {
				if (k > j) {
					break;
				}
				dp[i][j] += dp[i - 1][j - k];
			}
		}
	}
	cout << dp[n][m];
	return 0;
}
```

#### **5、排列**

题述：妞妞得到一个(1~n)的排列p1, p2, p3,...,pn, 听村里的老人牛牛说如果让这个排列变为:

对于所有的1 <= i <= n, 都满足pi ≠ i, 就可以获得Google Girl Hackathon的入场券。

妞妞仅允许的操作是: 交换排列中两个相邻的元素, 并且妞妞允许做这个操作任意次。

但是Google Girl Hackathon就快要开始了, 妞妞希望做最少的操作就使排列满足要求, 妞妞希望你能帮助她。

输入描述：输入包括两行, 第一行包括一个正整数n(2 <= n <= 10^5), 表示排列的长度和范围。 第二行包括n个正整数p1, p2, p3,...,pn, 即妞妞得到的排列, 保证是一个1~n的排列。

输出描述：输出一个整数, 表示妞妞需要的操作次数。

输入示例：5 

​		   1 4 3 5 2

输出示例：2

```c++
#include<iostream>
#include<algorithm>
#include<vector>
using namespace std;

int main()      
{
	int n;
	cin >> n;
	vector<int> vec(n+1);
	for (int i = 1; i <= n; i++) {
		cin >> vec[i];
	}
	int res = 0;
	for (int i = 1; i <= n; i++) {
		if (vec[i] == i) {
			res++;
			i++;
		}
	}
	cout << res;
	return 0;
}
```

#### **6、勇敢的妞妞**

题述：美丽的牛家庄受到了外星人的侵略, 勇敢的妞妞要上战场抵御侵略。在妞妞上战场前, 村长牛牛给了妞妞N件装备, 妞妞需要选择其中的K件,装备在身上提升自己的战斗力。每件装备有5种属性增幅值,对于第i件装备它的属性增幅值为(ri1, ri2, ri3, ri4, ri5), 分别代表该装备对不同的属性值增幅。当妞妞装备多件装备的时候,由于装备之前会互相影响, 对于每种属性值的增幅并不是所有装备该属性值之和, 而是该种属性值下所有装备中最大的属性值。而妞妞最终增加的战斗力为这5种属性值增幅之和。妞妞一定要保卫牛家庄, 所以她希望她能提升尽可能多的战斗力, 请你帮帮她计算她最多能增加多少战斗力。

输入描述：输入包括N+1行,第一行包括两个正整数N和K(1 <= N <= 10000, 1 <= K <= N), 分别表示一共有的装备数量和妞妞需要选择的装备数量。接下来的N行,每行5个整数ri1, ri2, ri3, ri4, ri5 (0 <= ri1, ri2, ri3, ri4, ri5 <= 10000)表示第i件装备的5种属性值增幅。

输出描述：输出一个整数,表示妞妞最多能增加的战斗力。

输入示例：4 2 

​		   30 30 30 30 0 

​		   50 0 0 0 0 

​		   0 50 0 50 10 

​		   0 0 50 0 20

输出示例：170

示例说明：妞妞要从4件装备中选取2件, 如果妞妞选择第1件和第3件装备,那么增加的战斗力为30 + 50 + 30 + 50 + 10 = 170, 这是最大的方案。

```c++
#include<iostream>
#include<algorithm>
#include<vector>
using namespace std;

//32个值分别为00000-11111的所有情况值大小  即对每一个装备的属性取或不取，求其和最大
int best[32] = { 0 };
int n, k;
int res = 0, max_s[5] = { 0 };
struct T {
	int ri[5];
};
void dfs(int cur,int flag, int sum) {
	if (cur == k) {
		res = max(res, sum);
	}
	else {
		for (int i = 0; i < 32; i++) {
			if (i&flag) {
				continue;
			}
			dfs(cur + 1, i | flag, sum + best[i]);
		}
	}
}
int main()      
{
	cin >> n >> k;
	vector<T> vec(n);
	for (int i = 0; i < n; i++) {
		T t;
		for (int m = 0; m < 5; m++) {
			cin >> t.ri[m];
			max_s[i] = max(max_s[i], t.ri[i]);
		}
		vec[i] = t;
		for (int j = 1; j < 32; j++) {
			int tmp = 0;
			for (int m = 0; m < 5; m++) {
				if (j&(1 << m)) {
					tmp += vec[i].ri[m];
				}
			}
			
			best[j] = max(best[j], tmp);
		}
	}
	if (k >= 5) {
		for (int i = 0; i < 5; i++) {
			res += max_s[i];
		}
	}
	else {
		dfs(0, 0, 0);
	}
	cout << res;
	return 0;
}
```

