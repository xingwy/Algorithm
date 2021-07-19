### **LEETCODE 481-500**

#### **[483. Smallest Good Base](https://leetcode-cn.com/problems/smallest-good-base/)**

Problem：
Given an integer n represented as a string, return the smallest good base of n.

We call k >= 2 a good base of n, if all digits of n base k are 1's.

Example：

```markdown
Input: n = "13"
Output: "3"
```

Explanation: 13 base 3 is 111.

```js
/**
 * @param {string} n
 * @return {string}
 */
var smallestGoodBase = function(n) {
    let now = BigInt(n);
    let MAX = now + 10n;
    let pow = function(k,n) {
        let t = 1n;
        for (let i=1n; i<n; i++) {
            t = t*k;
            if ((t - 1n) / (k - 1n) > now) {
                return (k - 1n) * MAX + 1n;
            }
        }
        return t;
    }
    let cal = function(k, n) {
        return (pow(k, n + 1n) - 1n) / (k - 1n);
    }
    for (let i=55n; i>=3n; i--) {
        let l = 2n;
        let n_t = Math.min(Math.ceil(Math.pow(Number(String(now)), 1/Number(String(i - 1n)))).toString(), Number(String(n))).toString();
        let r = BigInt(n_t);
        while(r >= l) {
            let o = l + r;
            if(o%2n != 0n){
                o += 1n;
            }
            let mid = o / 2n;
            let c = cal(mid, i);
            if (c == now) {
                return String(mid);
            } else {
                if (c >= n) {
                    r = mid - 1n;
                } else {
                    l = mid + 1n;
                }
            }
        }
    }
    return (now-1n).toString();
};
```

#### [485. Max Consecutive Ones](https://leetcode-cn.com/problems/max-consecutive-ones/)

Problem：

Given a binary array nums, return the maximum number of consecutive 1's in the array.

Example：

```markdown
Input: nums = [1,1,0,1,1,1]
Output: 3
```

Explanation: The first two digits or the last three digits are consecutive 1s. The maximum number of consecutive 1s is 3.

```js
/**
 * @param {number[]} nums
 * @return {number}
 */
var findMaxConsecutiveOnes = function(nums) {
    let cnt = 0;
    let max = 0;
    for (let i=0; i<nums.length; i++) {
        if (nums[i] == 1) {
            cnt++;
            max = Math.max(cnt, max);
        } else {
            cnt = 0;
        }
    }
    return max;
};
```

#### **[491. Increasing Subsequences](https://leetcode-cn.com/problems/increasing-subsequences/)**

Problem：

Given an integer array nums, return all the different possible increasing subsequences of the given array with at least two elements. You may return the answer in any order.

The given array may contain duplicates, and two equal integers should also be considered a special case of increasing sequence.

Example：

```markdown
Input: nums = [4,6,7,7]
Output: [[4,6],[4,6,7],[4,6,7,7],[4,7],[4,7,7],[6,7],[6,7,7],[7,7]]
```

```js
/**
 * @param {number[]} nums
 * @return {number[][]}
 */
var findSubsequences = function(nums) {
    let total = [];
    let dfs = function(start, res) {
        if(res.length >= 2) total.push([].concat(res));
        let _map = new Set();
        for (let i=start; i<nums.length; i++) {
            if (_map.has(nums[i]) || nums[i] < res[res.length-1]) continue;
            // 选取此值
            _map.add(nums[i]);
            res.push(nums[i]);
            dfs(i+1, res);
            // 回溯
            res.pop();
        }
    }
    dfs(0, []);
    return total;
};
```

#### **[492. Construct the Rectangle](https://leetcode-cn.com/problems/construct-the-rectangle/)**

Problem：

A web developer needs to know how to design a web page's size. So, given a specific rectangular web page’s area, your job by now is to design a rectangular web page, whose length L and width W satisfy the following requirements:

1. The area of the rectangular web page you designed must equal to the given target area.

2. The width W should not be larger than the length L, which means L >= W.

3. The difference between length L and width W should be as small as possible.


Return an array [L, W] where L and W are the length and width of the web page you designed in sequence.

Example：

```markdown
Input: area = 4
Output: [2,2]
```

Explanation: The target area is 4, and all the possible ways to construct it are [1,4], [2,2], [4,1]. 
But according to requirement 2, [1,4] is illegal; according to requirement 3,  [4,1] is not optimal compared to [2,2]. So the length L is 2, and the width W is 2.

```js
/**
 * @param {number} area
 * @return {number[]}
 */
var constructRectangle = function(area) {
    let l = Math.ceil(Math.sqrt(area));
    while (area%l != 0) l++;
    return [l, area/l];
};
```

#### **[494. Target Sum](https://leetcode-cn.com/problems/target-sum/)**

Problem：

You are given an integer array nums and an integer target.

You want to build an expression out of nums by adding one of the symbols '+' and '-' before each integer in nums and then concatenate all the integers.

- For example, if nums = [2, 1], you can add a '+' before 2 and a '-' before 1 and concatenate them to build the expression "+2-1".


Return the number of different expressions that you can build, which evaluates to target.

Example：

```markdown
Input: nums = [1,1,1,1,1], target = 3
Output: 5
```

Explanation: There are 5 ways to assign symbols to make the sum of nums be target 3.
-1 + 1 + 1 + 1 + 1 = 3
+1 - 1 + 1 + 1 + 1 = 3
+1 + 1 - 1 + 1 + 1 = 3
+1 + 1 + 1 - 1 + 1 = 3
+1 + 1 + 1 + 1 - 1 = 3

```js
/**
 * @param {number[]} nums
 * @param {number} S
 * @return {number}
 */
var findTargetSumWays = function(nums, S) {
    let mapList = [new Map(), new Map];
    let i = 0
    for (; i<nums.length; i++) {
        if (i == 0) {
            let _v1 = mapList[i%2].get(0 + nums[i]) || 0;
            mapList[i%2].set(0 + nums[i], 1 + _v1);
            let _v2 = mapList[i%2].get(0 - nums[i]) || 0;
            mapList[i%2].set(0 - nums[i], 1 + _v2);
        } else {
            mapList[(i+1)%2].forEach((v, k) => {
                let _v1 = mapList[i%2].get(k + nums[i]) || 0;
                mapList[i%2].set(k + nums[i], v + _v1);

                let _v2 = mapList[i%2].get(k - nums[i]) || 0;
                mapList[i%2].set(k - nums[i], v + _v2);
            })
            mapList[(i+1)%2].clear();
        }
    }
    return mapList[(i+1)%2].get(S) || 0;
};
```

