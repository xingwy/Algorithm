### **LEETCODE 221-240**

#### **[213. House Robber II](https://leetcode-cn.com/problems/house-robber-ii/)**

Problem：

You are a professional robber planning to rob houses along a street. Each house has a certain amount of money stashed. All houses at this place are arranged in a circle. That means the first house is the neighbor of the last one. Meanwhile, adjacent houses have a security system connected, and it will automatically contact the police if two adjacent houses were broken into on the same night.

Given an integer array nums representing the amount of money of each house, return the maximum amount of money you can rob tonight without alerting the police.

Example：

```markdown
Input: nums = [2,3,2]
Output: 3
```

Explanation: You cannot rob house 1 (money = 2) and then rob house 3 (money = 2), because they are adjacent houses.

```js
/**
 * @param {number[]} nums
 * @return {number}
 */
var rob = function(nums) {
    if (nums.length <= 0) {
        return 0;
    }
    if (nums.length == 1) {
        return nums[0];
    }
    let initer = function(nums1) {
        let dp = [[0],[0]];
        for (let i=0; i<nums1.length; i++) {
            if (i == 0) {
                dp[0][i] = nums1[i];
                dp[1][i] = 0;
            } else {
                dp[0][i] = dp[1][i-1] + nums1[i];
                dp[1][i] = Math.max(dp[0][i-1], dp[1][i-1]);
            }
        }
        return Math.max(dp[0][nums1.length-1], dp[1][nums1.length-1]);
    }
    let v1 = [].concat(nums);
    v1.shift();
    nums.pop();
    return Math.max(initer(v1), initer(nums));
};
```



