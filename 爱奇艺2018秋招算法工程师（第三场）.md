### **爱奇艺2018秋招算法工程师（第三场）**

#### **1、缺失的括号**

题述：一个完整的括号字符串定义规则如下: 1、空字符串是完整的。 2、如果s是完整的字符串，那么(s)也是完整的。 3、如果s和t是完整的字符串，将它们连接起来形成的st也是完整的。 例如，"(()())", ""和"(())()"是完整的括号字符串，"())(", "()(" 和 ")"是不完整的括号字符串。 牛牛有一个括号字符串s,现在需要在其中任意位置尽量少地添加括号,将其转化为一个完整的括号字符串。请问牛牛至少需要添加多少个括号。

输入描述：输入包括一行,一个括号序列s,序列长度length(1 ≤ length ≤ 50). s中每个字符都是左括号或者右括号,即'('或者')'.

输出描述：输出一个整数,表示最少需要添加的括号数

输入示例：(()(()

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
	int depth = 0, res = 0;
	for (int i = 0; i < str.size(); i++) {
		if (str[i] == '(') {
			depth++;
		}
		else if (str[i] == ')') {
			depth--;
		}
		if (depth < 0) {
			depth = 0;
			res++;
		}
	}
	cout << res + depth;
	return 0;
}
```

#### **2、最后一位**

题述：牛牛选择了一个正整数X,然后把它写在黑板上。然后每一天他会擦掉当前数字的最后一位,直到他擦掉所有数位。 在整个过程中,牛牛会把所有在黑板上出现过的数字记录下来,然后求出他们的总和sum. 例如X = 509, 在黑板上出现过的数字依次是509, 50, 5, 他们的和就是564. 牛牛现在给出一个sum,牛牛想让你求出一个正整数X经过上述过程的结果是sum.

输入描述：输入包括正整数sum(1 ≤ sum ≤ 10^18)

输出描述：输出一个正整数,即满足条件的X,如果没有这样的X,输出-1

输入示例：564

输出示例：509 

```c++
#include<iostream>
#include<algorithm>
using namespace std;

long long getSum(long long x) {
	long long sum = 0;
	while (x) {
		sum += x;
		x /= 10;
	}
	return sum;
}
int main()      
{
	//思路分析：对于一个数abc,其经规则算出后答案sum是大于于(a+i)(b+j)(c+k)、
	//所以本题的考点是二分查找
	long long x, sum;
	cin >> sum;
	long long l = 0, r = sum;
	while (r >= l) {
		long long mid = (r + l) / 2;
		if (getSum(mid) == sum) {
			x = mid;
			break;
		}
		else if (getSum(mid) > sum) {
			r = mid - 1;
		}
		else {
			l = mid + 1;
		}
	}
	cout << x;
	return 0;
}
```

#### **3、冒泡排序**

题述：牛牛学习了冒泡排序,并写下以下冒泡排序的伪代码,注意牛牛排序的数组a是从下标0开始的。 

```c++
BubbleSort(a): 
	Repeat length(a)-1 
        times: For every i from 0 to length(a) - 2
            If a[i] > a[i+1] 
            then: Swap a[i] and a[i+1] 
```

牛牛现在要使用上述算法对一个数组A排序。 在排序前牛牛允许执行最多k次特定操作(可以不使用完),每次特定操作选择一个连续子数组,然后对其进行翻转,并且k次特定操作选择的子数组不相交。 例如A = {1, 2, 3, 4, 5, 6, 7}, k = 1,如果牛牛选择的子数组是[2,4] ,那么翻转之后的数组变为A = {1, 2, 5, 4, 3, 6, 7}。 牛牛知道冒泡排序的效率一定程度上取决于Swap操作次数,牛牛想知道对于一个数组A在进行k次特定操作之后,再进行上述冒泡排序最少的Swap操作次数是多少? 

输入描述：输入包括两行,第一行包括两个正整数n和k(2 ≤ n ≤ 50, 1 ≤ k ≤ 50),表示数组的长度和允许最多的特定操作次数。 第二行n个正整数A[i] (1 ≤ A[i] ≤ 1000),表示数组内的元素,以空格分割。

输出描述：输出一个整数,表示在执行最多k次特定操作之后,对数组进行上述冒泡排序需要的Swap操作次数。

输入示例：3 2 

​		   2 3 1

输出示例：1

```c++
#include<iostream>
#include<algorithm>
#include<vector>
using namespace std;

//得到容器的逆序数
int getReverseNum(vector<int>& v,int cur) {
	int count = 0;
	for (int i = cur; i < v.size(); i++) {
		for (int j = 0; j < i; j++) {
			if (v[i] < v[j]) {
				count++;
			}
		}
	}
	return count;
}

int main()      
{
	int n, k;
	cin >> n >> k;
	vector<int> vec(n);
	for (int i = 0; i < n; i++) {
		cin >> vec[i];
	}
	//本质求在k次操作之后数组的最小逆序数   笔者在这题卡住了，此代码是示例代码   
    //因式含义： dp[i][j]表示下标i至末尾有j次变化机会的最小逆序数
	vector<vector<int>> dp(n + 1, vector<int>(k + 1, 0));
	for (int i = n - 1; i >= 0; i--) {
		for (int j = 0; j <= k; j++) {
			vector<int> tmp1(vec.begin(), vec.begin() + i + 1);
			dp[i][j] = getReverseNum(tmp1, i) + dp[i + 1][k];
			if (j > 0) {
				for (int l = i + 1; l < n; l++) {
					vector<int> tmp2(vec.begin(), vec.begin() + l + 1);
					reverse(tmp2.begin() + i, tmp2.begin() + l + 1);
					dp[i][j] = min(dp[i][j], getReverseNum(tmp2, i) + dp[l + 1][j - 1]);
				}
			}
		}
	}
	cout << dp[0][k];
	return 0;
}
```



