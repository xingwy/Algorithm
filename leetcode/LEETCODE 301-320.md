### **LEETCODE 301-320**

#### **[312. Burst Balloons](https://leetcode-cn.com/problems/burst-balloons/)**

Problem：

You are given n balloons, indexed from 0 to n - 1. Each balloon is painted with a number on it represented by an array nums. You are asked to burst all the balloons.

If you burst the ith balloon, you will get nums[i - 1] * nums[i] * nums[i + 1] coins. If i - 1 or i + 1 goes out of bounds of the array, then treat it as if there is a balloon with a 1 painted on it.

Return the maximum coins you can collect by bursting the balloons wisely.

Example：

```js
Input: nums = [3,1,5,8]
Output: 167
```

Explanation:
nums = [3,1,5,8] --> [3,5,8] --> [3,8] --> [8] --> []
coins =  3*1*5    +   3*5*8   +  1*3*8  + 1*8*1 = 167

```js
/**
 * @param {number[]} nums
 * @return {number}
 */
var maxCoins = function(nums) {
    // dp[i][j]  [i,j]的结果
    let len = nums.length;
    nums.unshift(1);
    nums.push(1);
    let dp = [];
    for (let i=1; i<=len; i++) {
        dp[i] = [];
        dp[i][i] = nums[i-1]*nums[i]*nums[i+1];
    }
    // range表示区间长度
    for (let range=2; range<=len; range++) {
        console.log(range)
        for (let i=1; i<=len-range+1; i++) {
            let j = i+range-1;
            let max = -Infinity;
            for (let k=i; k<=j; k++) {
                // 假设nums[k]为最后处理的气球 dp[i,j] = dp[i,k-1] + dp[k+1, j] + nums[k]*nums[i-1]*nums[j+1];
                let prev = (i == k ? 0 : dp[i][k-1]);
                let tail = (j == k ? 0 : dp[k+1][j]);
                let curr = ((k == i && k == j) ? 0 : nums[k]*nums[i-1]*nums[j+1]);
                max = Math.max(prev + tail + curr, max);
            }
            if (dp[i] == undefined) {
                dp[i] = [];
            }
            dp[i][j] = max;
        }
    }
    return dp[1][len];
};

```











