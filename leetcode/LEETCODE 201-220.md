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

#### **[215. Kth Largest Element in an Array](https://leetcode-cn.com/problems/kth-largest-element-in-an-array/)**

Problem：
Given an integer array `nums` and an integer `k`, return *the* `kth` *largest element in the array*.

Note that it is the `kth` largest element in the sorted order, not the `kth` distinct element.

Example：

```markdown
Input: nums = [3,2,1,5,6,4], k = 2
Output: 5
```

```js
/**
 * @param {number[]} nums
 * @param {number} k
 * @return {number}
 */
var findKthLargest = function(nums, k) {
    nums.sort((a,b) => (b -a));
    return nums[k-1];
};
```

#### **[216. Combination Sum III](https://leetcode-cn.com/problems/combination-sum-iii/)**

Problem：

Find all valid combinations of k numbers that sum up to n such that the following conditions are true:

- Only numbers 1 through 9 are used.

- Each number is used at most once.

Return a list of all possible valid combinations. The list must not contain the same combination twice, and the combinations may be returned in any order.

Example：

```js
Input: k = 3, n = 9
Output: [[1,2,6],[1,3,5],[2,3,4]]
```

Explanation:
1 + 2 + 6 = 9
1 + 3 + 5 = 9
2 + 3 + 4 = 9
There are no other valid combinations.

```js
/**
 * @param {number} k
 * @param {number} n
 * @return {number[][]}
 */
var combinationSum3 = function(k, n) {
    let res = [];
    let dfs = function (i, len, sum, total) {
        if (len > k || sum > n) {
            return;
        }
        if (len == k) {
            if (sum == n) {
                res.push([].concat(total));
            }
        } else {
            for (let j=i+1; j<=9; j++) {
                total.push(j);
                dfs(j, len + 1, sum + j, total);
                total.pop();
            }
        }
    }
    dfs(0,0,0,[]);
    return res;
};
```

#### **[217. Contains Duplicate](https://leetcode-cn.com/problems/contains-duplicate/)**

Problem：

Given an integer array nums, return true if any value appears at least twice in the array, and return false if every element is distinct.

Example：

```markdown
Input: nums = [1,2,3,1]
Output: true
```

```js
/**
 * @param {number[]} nums
 * @return {boolean}
 */
var containsDuplicate = function(nums) {
    let _map = new Set();
    for (let n of nums) {
        if (_map.has(n)) {
            return true;
        } else {
            _map.add(n);
        }
    }
    return _map.size != nums.length;
};
```

#### **[218. 天际线问题](https://leetcode-cn.com/problems/the-skyline-problem/)**

[problem](https://github.com/xingwy/Algorithm/blob/master/common/%E5%A4%A9%E9%99%85%E7%BA%BF%E9%97%AE%E9%A2%98.md)



