### **GarsiaWachs算法**

原题描述：在一条直线上摆着n堆石子，现要将石子有次序地合并成一堆。规定每次只能选取相邻的两堆进行合并，合并的权重等于合并的新石堆数量。怎么合并使总权重最小？

示例如下：

```markdown
输入： data = [4,5,9,4]
第一次合并：[4+5,9,4] => [9,9,4]
第二次合并：[9,9+4] => [9,13]
第三次合并：[9+13] => [22]
输出：9+13+22 => 44
```

分析一下，如果有n堆，那么将合并n-1次，全局最优解包括每一次合并的子问题最优解。那么，我们可以使用动态规划的思想去分解这个问题。

设dp[i, j]代表合并i到j堆之间的最少权重。那dp[1,n]即使全局最小权重。

设data[i]代表第i堆的石子个数。

先看dp，对于dp的状态，以区间的长度进行分析（简称区间长度为度），如下：

- 度为1，j=i，那么dp[i,i] = 0，因为无需合并。

- 度为2，j=i+1，那么dp[i,i+1] = data[i]+data[i+1]。只有这一种合并方式。

- 度为3，j=i+2，那么dp[i,i+2]将会有两种合并，先合并data[i]和data[i+1]，或先合并data[i+1]和data[i+2]。

  令lastWeight = data[i]+data[i+1]+data[i+2];（最后一次合并的结果）

  令res1 = dp[i,i+1] + dp[i+2,i+2] + lastWeight ;（先合并data[i]和data[i+1]）
  令res2 = dp[i,i] + dp[i+1,i+2] + lastWeight ;（先合并data[i+1]和data[i+2]）

- ···

- 以此类推，当度为j-i的时候，求dp[i,j]，可以设k∈[i, j)（左闭右开）。那么，求dp[i,j]将等于dp[i,k]+dp[k+1,j]+data[i]+data[i+1]+ ··· +data[j],为了方便，引入lastWeight[i,j] 为 data从i到j的堆和。

所以，得出方程式为dp[i,j] = Min{dp[i,k]+dp[k+1,j]+lastWeight[i,j]} k∈[i, j)。方程实现如下：

```js
let handler = function(data) {
    let len = data.length;
    let dp = [];
    let lastWeight;

    // 构造初始度为1数据
    for (let i=0; i<len; i++) {
        if (!dp[i]) {
            dp[i] = [];
        }
        dp[i][i] = 0;
    }

    // 构造初始度为2数据
    for (let i=0; i<len-1; i++) {
        if (!dp[i]) {
            dp[i] = [];
        }
        dp[i][i+1] = data[i]+data[i+1];
    }

    // 推理数据 i代表度
    for (let w=2; w<len-1; w++) {
        for (let i=0; i<len-w+1; i++) {
            let j = i+w-1;
            // 构造dp[i][j]
            let lastWeight = 0;

            // 最后一次合并总权重
            for (let t=i; t<=j; t++) {
                lastWeight += data[t];
            }
            let min = Infinity;
            // 定义区分点k
            for (let k=i; k<j; k++) {
                min = Math.min(min, dp[i][k] + dp[k+1][j] + lastWeight);
            }
        }
    }
    return dp[0][n-1];
}
```

dp采用二维数组，空间和时间复杂度都为O(n^2)，对于少量数据可以解答此问题，当数据量扩展到10^5时，那么空间和时间的占用将会显得无法满足。

