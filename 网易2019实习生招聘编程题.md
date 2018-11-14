### **网易2019实习生招聘编程题**

#### **1、牛牛找工作**

题述：为了找到自己满意的工作，牛牛收集了每种工作的难度和报酬。牛牛选工作的标准是在难度不超过自身能力值的情况下，牛牛选择报酬最高的工作。在牛牛选定了自己的工作后，牛牛的小伙伴们来找牛牛帮忙选工作，牛牛依然使用自己的标准来帮助小伙伴们。牛牛的小伙伴太多了，于是他只好把这个任务交给了你。 输入描述: 每个输入包含一个测试用例。 每个测试用例的第一行包含两个正整数，分别表示工作的数量N(N<=100000)和小伙伴的数量M(M<=100000)。 接下来的N行每行包含两个正整数，分别表示该项工作的难度Di(Di<=1000000000)和报酬Pi(Pi<=1000000000)。 接下来的一行包含M个正整数，分别表示M个小伙伴的能力值Ai(Ai<=1000000000)。 保证不存在两项工作的报酬相同。

输出描述：对于每个小伙伴，在单独的一行输出一个正整数表示他能得到的最高报酬。一个工作可以被多个人选择。

输入示例：3  3

​                   1  100

​		   10  1000

​                   1000000000 1001

​		   9 10 1000000000

输出示例：100 1000 1001

```c++
#include<iostream>
#include <algorithm>
#include <vector>
using namespace std;

struct Job {
	int Di;
	int Pi;
};
struct Worker {
	int Ai;
	int pos_i;
};
bool comparison_Job(Job a, Job b) {
	return a.Di < b.Di;
}
bool comparison_Worker(Worker a, Worker b) {
	return a.Ai < b.Ai;
}

int main()
{
	int	n, m;
	cin >> n >> m;
	vector<Job> jobs;
	vector<Worker> workers;
	int *earn = new int[m];
	//录入工作信息集合
	for (int i = 0; i < n; i++) {
		int pi, di;
		cin >> di >> pi;
		Job job;
		job.Di = di;
		job.Pi = pi;
		jobs.push_back(job);
	}
	for (int i = 0; i < m; i++) {
		int ai;
		cin >> ai;
		Worker worker;
		worker.Ai = ai;
		worker.pos_i = i;
		workers.push_back(worker);
	}
	//对jobs按照Di升序排序
	sort(jobs.begin(), jobs.end(), comparison_Job);
	sort(workers.begin(), workers.end(), comparison_Worker);
	//剔除 Di>Dj 且 Pi<Pj的情况  
	for (int i = 0; i < jobs.size() - 1; i++) {
		if (jobs[i].Pi >= jobs[i + 1].Pi)
			jobs[i+1].Pi = jobs[i].Pi;
	}
	int pos = 0;
	for (int i = 0; i < workers.size(); i++) {
		while (jobs[pos].Di <= workers[i].Ai) {
			pos++;
			if (pos >= jobs.size()) {
				pos = jobs.size();
				break;
			}
		}
		if (pos >= 1) {
			earn[workers[i].pos_i] = jobs[pos-1].Pi;
		}
		else {
			earn[workers[i].pos_i] = 0;
		}
	}
	for (int i = 0; i < m; i++) {
		cout << earn[i] << " ";
	}
	return 0;
}
```

#### **2、被3整除**

题述：小Q得到一个神奇的数列: 1, 12, 123,...12345678910,1234567891011...。并且小Q对于能否被3整除这个性质很感兴趣。小Q现在希望你能帮他计算一下从数列的第l个到第r个(包含端点)有多少个数可以被3整除。

输入描述：输入包括两个整数l和r(1 <= l <= r <= 1e9), 表示要求解的区间两端。

输出描述：输出一个整数, 表示区间内能被3整除的数字个数。 

输入示例：2 5 

输出示例：3

说明 12, 123, 1234, 12345... 其中12, 123, 12345能被3整除。

```c++
#include<iostream>
using namespace std;

//找出规律集合
int getCount(int r) {
	return int(r / 3) * 2 + (r % 3 == 2 ? 1 : 0);
}
int main()
{
	int l, r;
	cin >> l >> r;
	int count;
	count = getCount(r) - getCount(l) + (l % 3 == 1 ? 0 : 1);
	cout << count;
	return 0;
}
```

#### **3、安置路灯**

题述：小Q正在给一条长度为n的道路设计路灯安置方案。为了让问题更简单,小Q把道路视为n个方格,需要照亮的地方用'.'表示, 不需要照亮的障碍物格子用'X'表示。小Q现在要在道路上设置一些路灯, 对于安置在pos位置的路灯, 这盏路灯可以照亮pos - 1, pos, pos + 1这三个位置。小Q希望能安置尽量少的路灯照亮所有'.'区域, 希望你能帮他计算一下最少需要多少盏路灯。

输入描述：输入的第一行包含一个正整数t(1 <= t <= 1000), 表示测试用例数 接下来每两行一个测试数据, 第一行一个正整数n(1 <= n <= 1000),表示道路的长度。 第二行一个字符串s表示道路的构造,只包含'.'和'X'。

输出描述：对于每个测试用例, 输出一个正整数表示最少需要多少盏路灯。 

输入示例：2

​		   3   .X. 

​                   11   ...XX....XX 

输出示例：1 3

```c++
#include<iostream>
#include <algorithm>
#include <string>
#include <vector>
using namespace std;

struct Road {
	int len;
	string ground;
};
int main()        //求出'.'的段数级每段的长度
{
	int n;
	cin >> n;
	vector<Road> vec;
	vector<int> res;
	for (int i = 0; i < n; i++) {
		Road r;
		cin >> r.len >> r.ground;
		vec.push_back(r);
	}
	for (int i = 0; i < n; i++) {
		res.push_back(0);
		cout << vec[i].ground << endl;
		for (int j = 0; j < vec[i].len;) {
			int count = 0;
			while (vec[i].ground[j] == 'X') {
				
				j++;
			}
			while (vec[i].ground[j] == '.') {
				count++;
				j++;
			}
			res[i] += int((count + 2) / 3);
		}
		cout << endl;
	}
	for (int i = 0; i < res.size(); i++) {
		cout << res[i] << " ";
	}
	return 0;
}
```

#### **4、迷路的牛牛**

题述：牛牛去犇犇老师家补课，出门的时候面向北方，但是现在他迷路了。虽然他手里有一张地图，但是他需要知道自己面向哪个方向，请你帮帮他。 

输入描述：每个输入包含一个测试用例。 每个测试用例的第一行包含一个正整数，表示转方向的次数N(N<=1000)。 接下来的一行包含一个长度为N的字符串，由L和R组成，L表示向左转，R表示向右转。

输出描述：输出牛牛最后面向的方向，N表示北，S表示南，E表示东，W表示西。

输入示例：3  LRR

输出示例：E

```c++
#include<iostream>
#include <string>
using namespace std;

int main()
{
	int n;
	string str;
	cin >> n >> str;      //变换朝向状态
	char forward[4] = { 'N','W','S','E' };
	int pos = 0;
	for (int i = 0; i < n; i++) {
		if (str[i] == 'L') {
			pos = (pos + 1) % 4;
		}
		else if (str[i] == 'R') {
			pos = (pos + 3) % 4;
		}
	}
	cout << forward[pos];
	return 0;
}
```

#### **5、数对**

题述：牛牛以前在老师那里得到了一个正整数数对(x, y), 牛牛忘记他们具体是多少了。但是牛牛记得老师告诉过他x和y均不大于n, 并且x除以y的余数大于等于k。 牛牛希望你能帮他计算一共有多少个可能的数对。

输入描述：输入包括两个正整数n,k(1 <= n <= 10^5, 0 <= k <= n - 1)。

输出描述：对于每个测试用例, 输出一个正整数表示可能的数对数量。

输入示例：5 2

输出示例：7

```c++
#include<iostream>
using namespace std;

int main()
{
	int n, k;
	cin >> n >> k;
	int count = 0;
	for (int y = k+1; y <= n; y++) {
		//求每个y下的x个数
		count = count + int(n / y)*(y - k) + (n%y >= k ? n % y - k + 1 : 0);
	}
	cout << count;
	return 0;
}
```

#### **6、矩阵重叠**

题述：平面内有n个矩形, 第i个矩形的左下角坐标为(x1[i], y1[i]), 右上角坐标为(x2[i], y2[i])。如果两个或者多个矩形有公共区域则认为它们是相互重叠的(不考虑边界和角落)。请你计算出平面内重叠矩形数量最多的地方,有多少个矩形相互重叠。

输入描述：输入包括五行。 第一行包括一个整数n(2 <= n <= 50), 表示矩形的个数。 第二行包括n个整数x1[i] (-10^9 <= x1[i] <= 10^9),表示左下角的横坐标。 第三行包括n个整数y1[i] (-10^9 <= y1[i] <= 10^9),表示左下角的纵坐标。 第四行包括n个整数x2[i] (-10^9 <= x2[i] <= 10^9),表示右上角的横坐标。 第五行包括n个整数y2[i] (-10^9 <= y2[i] <= 10^9),表示右上角的纵坐标。

输出描述：输出一个正整数, 表示最多的地方有多少个矩形相互重叠,如果矩形都不互相重叠,输出1。

输入示例：2 

​		   0 90 

​                   0 90 

​                   100 200 

​		   100 200

输出示例：2

```c++
#include<iostream>
#include <algorithm>
#include <vector>
using namespace std;

struct Rect {
	int lpos_x;
	int lpos_y;
	int rpos_x;
	int rpos_y;
	int count;
};
void swap(Rect& a, Rect& b) {
	Rect tmp;
	tmp = a;
	a = b;
	b = tmp;
}
void show(Rect item) {
	cout << item.lpos_x << " " << item.lpos_y << " " << item.rpos_x << " " << item.rpos_y << " " << item.count << endl;
}
Rect getCommonRect(Rect a, Rect b) {
	Rect item;
	item.count = 0;
	if (a.lpos_x > b.lpos_x) 
		swap(a, b);

	if (a.rpos_y < b.lpos_y)
		return item;
	if (a.rpos_x < b.lpos_x)
		return item;
	if (a.lpos_y > b.rpos_y)
		return item;

	item.lpos_x = max(a.lpos_x, b.lpos_x);
	item.lpos_y = max(a.lpos_y, b.lpos_y);
	item.rpos_x = min(a.rpos_x, b.rpos_x);
	item.rpos_y = min(a.rpos_y, b.rpos_y);
	item.count = max(a.count, b.count) + 1;
	return item;
}
int main()           //动态更新一个矩阵容器  容器包含已知的所有矩阵（包括重叠的）
{
	int n;
	cin >> n;
	vector<Rect> rects;
	vector<Rect> res;
	int *p = new int[4 * n];
	for (int i = 0; i < 4*n; i++) {
		cin >> p[i];
	}
	for (int i = 0; i < n; i++) {
		Rect rect;
		rect.lpos_x = p[i];
		rect.lpos_y = p[n + i];
		rect.rpos_x = p[2 * n + i];
		rect.rpos_y = p[3 * n + i];
		rect.count = 1;
		rects.push_back(rect);
	}
	res.push_back(rects[0]);
	for (int i = 1; i < n; i++) {
		int len = res.size();
		for (int j = 0; j < len; j++) {
			Rect item = getCommonRect(rects[i], res[j]);
			if (item.count == 0)
				continue;
			res.push_back(item);
		}
		res.push_back(rects[i]);
	}
	int result = 0;
	for (int i = 0; i < res.size(); i++) {
		result = max(result, res[i].count);
	}
	cout << result;
	return 0;
}
```

#### **7、牛牛的闹钟**

题述：牛牛总是睡过头，所以他定了很多闹钟，只有在闹钟响的时候他才会醒过来并且决定起不起床。从他起床算起他需要X分钟到达教室，上课时间为当天的A时B分，请问他最晚可以什么时间起床 

输入描述：每个输入包含一个测试用例。 每个测试用例的第一行包含一个正整数，表示闹钟的数量N(N<=100)。 接下来的N行每行包含两个整数，表示这个闹钟响起的时间为Hi(0<=A<24)时Mi(0<=B<60)分。 接下来的一行包含一个整数，表示从起床算起他需要X(0<=X<=100)分钟到达教室。 接下来的一行包含两个整数，表示上课时间为A(0<=A<24)时B(0<=B<60)分。 数据保证至少有一个闹钟可以让牛牛及时到达教室。

输出描述：输出两个整数表示牛牛最晚起床时间。

输入示例：3    5 0  6 0  7 0 

​		   59 

​		   6 59

输出示例：6 0

```c++
#include<iostream>
#include <algorithm>
#include <vector>
using namespace std;

int main()
{
	int n,x,startTime,endTime;
	vector<int> vec;
	cin >> n;
	for (int i = 0; i < n; i++) {
		int hour, minute;
		cin >> hour >> minute;
		vec.push_back((hour % 24) * 60 + minute);
	}
	int h, m;
	cin >> x >> h >> m;
	endTime = (h % 24) * 60 + m;
	startTime = (endTime - x + 24 * 60) % (24 * 60);
	//闹钟排序
	sort(vec.begin(), vec.end());	
	int lateTime = 0;
	for (int i = 0; i < vec.size(); i++) {
		if (endTime - x >= 0) {
			if ((endTime - x) >= vec[i]) {
				lateTime = vec[i];
				continue;
			}
			break;
		}
		else {
			if ((endTime - x + 24 * 60) >= vec[i]) {
				lateTime = vec[i];
				continue;
			}
			break;
		}
	}
	int resH, resM;
	resH = lateTime / 60;
	resM = lateTime % 60;
	cout << resH << " " << resM;
	return 0;
}
```

#### **8、牛牛的背包问题**

题述：牛牛准备参加学校组织的春游, 出发前牛牛准备往背包里装入一些零食, 牛牛的背包容量为w。 牛牛家里一共有n袋零食, 第i袋零食体积为v[i]。 牛牛想知道在总体积不超过背包容量的情况下,他一共有多少种零食放法(总体积为0也算一种放法)。

输入描述：输入包括两行 第一行为两个正整数n和w(1 <= n <= 30, 1 <= w <= 2 * 10^9),表示零食的数量和背包的容量。 第二行n个正整数v[i] (0 <= v[i] <= 10^9),表示每袋零食的体积。

输出描述：输出一个正整数, 表示牛牛一共有多少种零食放法。

输入示例：3 10 1 2 4

输出示例：8

```c++
#include<iostream>
#include <algorithm>
#include <vector>
using namespace std;

int n, bag_w;
vector<int> vec;
int dps(int w, int curr) {
	if (curr == n) {
		return 1;
	}
	if ((w + vec[curr]) <= bag_w) {
		return dps(w, curr + 1) + dps(w + vec[curr], curr+1);
	}
    //剪枝
	else {
		return 1;
	}
}
int main()
{
	cin >> n >> bag_w;
	for (int i = 0; i < n; i++) {
		int item;
		cin >> item;
		vec.push_back(item);
	}
	sort(vec.begin(), vec.end());
	int res = 0;
	res = dps(0, 0);
	cout << res;
	return 0;
}
```