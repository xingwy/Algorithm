### **LEETCODE 481-500**

#### **[482. License Key Formatting](https://leetcode-cn.com/problems/license-key-formatting/)**

Problem：

You are given a license key represented as a string s that consists of only alphanumeric characters and dashes. The string is separated into n + 1 groups by n dashes. You are also given an integer k.

We want to reformat the string s such that each group contains exactly k characters, except for the first group, which could be shorter than k but still must contain at least one character. Furthermore, there must be a dash inserted between two groups, and you should convert all lowercase letters to uppercase.

Return the reformatted license key.

Example：

```markdown
Input: s = "5F3Z-2e-9-w", k = 4
Output: "5F3Z-2E9W"
```

Explanation: The string s has been split into two parts, each part has 4 characters.
Note that the two extra dashes are not needed and can be removed.

```js
/**
 * @param {string} S
 * @param {number} K
 * @return {string}
 */
var licenseKeyFormatting = function(S, K) {
    let _map = {};
    let offset = 'A'.charCodeAt() - 'a'.charCodeAt();
    for (let i='a'.charCodeAt(); i<='z'.charCodeAt(); i++) {
        _map[ String.fromCharCode(i)] = String.fromCharCode(i+offset)
    }
    let list = S.split("-").filter((v) => !!v).map((v) => {
        let t = "";
        for (let i=0; i<v.length; i++) {
            if (v[i].charCodeAt() <="z".charCodeAt() && v[i].charCodeAt()>="a".charCodeAt()) {
                t += _map[v[i]];
            } else {
                t += v[i];
            }
        }
        return t;
    });
    let left = "";
    let result = [];
    for (let i=list.length-1; i>=0; i--) {
        left = list[i] + left;
        while (left.length >= K) {
            result.push(left.substring(left.length - K));
            left = left.substring(0, left.length - K);
        }
    }
    if (left) {
        result.push(left);
    }
    return result.reverse().join("-");
};
```

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

#### **[495. Teemo Attacking](https://leetcode-cn.com/problems/teemo-attacking/)**

Problem：

Our hero Teemo is attacking an enemy Ashe with poison attacks! When Teemo attacks Ashe, Ashe gets poisoned for a exactly duration seconds. More formally, an attack at second t will mean Ashe is poisoned during the inclusive time interval [t, t + duration - 1]. If Teemo attacks again before the poison effect ends, the timer for it is reset, and the poison effect will end duration seconds after the new attack.

You are given a non-decreasing integer array timeSeries, where timeSeries[i] denotes that Teemo attacks Ashe at second timeSeries[i], and an integer duration.

Return the total number of seconds that Ashe is poisoned.

Example：

```markdown
Input: timeSeries = [1,4], duration = 2
Output: 4
```

Explanation: Teemo's attacks on Ashe go as follows:
- At second 1, Teemo attacks, and Ashe is poisoned for seconds 1 and 2.
- At second 4, Teemo attacks, and Ashe is poisoned for seconds 4 and 5.
Ashe is poisoned for seconds 1, 2, 4, and 5, which is 4 seconds in total.

```js
/**
 * @param {number[]} timeSeries
 * @param {number} duration
 * @return {number}
 */
var findPoisonedDuration = function(timeSeries, duration) {
    let total = timeSeries.length * duration;
    for (let i=1; i<timeSeries.length; i++) {
        if (timeSeries[i] < (timeSeries[i-1] + duration)) {
            total -= (timeSeries[i-1] + duration - timeSeries[i]);
        }
    }
    return total;
};
```

#### **[496. Next Greater Element I](https://leetcode-cn.com/problems/next-greater-element-i/)**

Problem：

The next greater element of some element x in an array is the first greater element that is to the right of x in the same array.

You are given two distinct 0-indexed integer arrays nums1 and nums2, where nums1 is a subset of nums2.

For each 0 <= i < nums1.length, find the index j such that nums1[i] == nums2[j] and determine the next greater element of nums2[j] in nums2. If there is no next greater element, then the answer for this query is -1.

Return an array ans of length nums1.length such that ans[i] is the next greater element as described above.

Example：

```markdown
Input: nums1 = [4,1,2], nums2 = [1,3,4,2]
Output: [-1,3,-1]
```

Explanation: The next greater element for each value of nums1 is as follows:
- 4 is underlined in nums2 = [1,3,4,2]. There is no next greater element, so the answer is -1.
- 1 is underlined in nums2 = [1,3,4,2]. The next greater element is 3.
- 2 is underlined in nums2 = [1,3,4,2]. There is no next greater element, so the answer is -1.

```js
/**
 * @param {number[]} nums1
 * @param {number[]} nums2
 * @return {number[]}
 */
var nextGreaterElement = function(nums1, nums2) {
    let res = [];
    for (let i=0; i<nums1.length; i++) {
        let start = false;
        let ind = -1;
        for (let j=0; j<nums2.length; j++) {
            if (nums2[j] == nums1[i]) {
                start = true
            }
            if (start && nums1[i] < nums2[j]) {
                ind = nums2[j];
                break;
            }
        }
        res.push(ind);
    }
    return res;
};
```

#### **[498. Diagonal Traverse](https://leetcode-cn.com/problems/diagonal-traverse/)**

Problem：

Given an m x n matrix mat, return an array of all the elements of the array in a diagonal order.

Example：

![diag1-grid](https://github.com/xingwy/Hugging-Algorithm/blob/master/images/diag1-grid.jpg)

```markdown
Input: mat = [[1,2,3],[4,5,6],[7,8,9]]
Output: [1,2,4,7,5,3,6,8,9]
```

```js
/**
 * @param {number[][]} matrix
 * @return {number[]}
 */
var findDiagonalOrder = function(matrix) {
    let m = matrix.length;
    if (!m) {
        return [];
    } 
    let n = matrix[0].length;
    if (!n) {
        return []
    }
    let index = 0;
    let needreverse  = false;
    let res = [];
    while (index <(m + n - 1)) {
        let i = Math.min(index, m - 1);
        let j = index - i;
        let tmp = [];
        while (true) {
            if (i < 0 || j >= n) {
                break;
            }
            tmp.push(matrix[i][j]);
            i--;
            j++;
        }
        if (needreverse) {
            tmp.reverse();
        }
        res.push(...tmp);
        needreverse = !needreverse;
        index++;
    }
    return res;
};
```

#### **[500. Keyboard Row](https://leetcode-cn.com/problems/keyboard-row/)**

Problem：

Given an array of strings words, return the words that can be typed using letters of the alphabet on only one row of American keyboard like the image below.

In the American keyboard:

- the first row consists of the characters "qwertyuiop",
- the second row consists of the characters "asdfghjkl", and
- the third row consists of the characters "zxcvbnm".

![keyboard](https://github.com/xingwy/Hugging-Algorithm/blob/master/images/keyboard.png)

Example：

```markdown
Input: words = ["Hello","Alaska","Dad","Peace"]
Output: ["Alaska","Dad"]
```

```js
/**
 * @param {string[]} words
 * @return {string[]}
 */
var findWords = function(words) {
    let t = [];
    let s = [];
    t[0] = "QWERTYUIOPqwertyuiop";
    t[1] = "ASDFGHJKLasdfghjkl";
    t[2] = "ZXCVBNMzxcvbnm";
    for (let i=0; i<3; i++) {
        s[i] = new Set();
        for (let j=0; j<t[i].length; j++) {
            s[i].add(t[i][j]);
        }
    }

    let res = words.filter((word) => {
        let r = true;
        if (!word) {
            return true;
        }
        let i=0;
        for (;i<3; i++) {
            if(s[i].has(word[0])) {
                break;
            }
        }
        for (let j=1; j<word.length; j++) {
            if(!s[i].has(word[j])) {
                return false;
            }
        }
        return true;
    })
    return res;
};
```

