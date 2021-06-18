### **LEETCODE 41-60**

#### **[41.First Missing Positive](https://leetcode-cn.com/problems/first-missing-positive/)**

Promise：Given an unsort interger array,find the smallest missing positive integer, your algorithm should run in O(n) time and uses constant space

Example 1:

```text
Input: [1,2,0]
Output: 3
```

Example 2:

```text
Input: [3,4,-1,1]
Output: 2
```

Example 3:

```text
Input: [7,8,9,11,12]
Output: 1
```

Solutions:

According to this promise content, we know that we need control our algorithm run in O(n), that meaning we can't sort the array. when we find the smallest interval, it must between 1 and the size of this array. so we only need sort or find the interval which meet the conditions.

Code:

```c++
class Solution {
public:
    int firstMissingPositive(vector<int>& nums) {
        int n = nums.size();
        for (int i = 0; i < n; ++i) {
            while (nums[i] > 0 && nums[i] <= n && nums[nums[i] - 1] != nums[i]) {
                swap(nums[i], nums[nums[i] - 1]);
            }
        }
        for (int i = 0; i < n; ++i) {
            if (nums[i] != i + 1) return i + 1;
        }
        return n + 1;
    }
};
```

#### **[42.Trapping Rain Water](https://leetcode-cn.com/problems/trapping-rain-water/)**

Problem：

Given n non-negative integers representing an elevation map where the width of each bar is 1, compute how much water it can trap after raining.

![rainwatertrap_42](https://github.com/xingwy/Hugging-Algorithm/blob/master/images/rainwatertrap_42.png)

Example：

```markdown
Input: height = [0,1,0,2,1,0,1,3,2,1,2,1]
Output: 6
```

Explanation: The above elevation map (black section) is represented by array [0,1,0,2,1,0,1,3,2,1,2,1]. In this case, 6 units of rain water (blue section) are being trapped.

```js

/**
 * @param {number[]} height
 * @return {number}
 */
var trap = function(height) {
    let leftWall = 0;
    let rightWall = 0;
    let sum = 0;
    let i = 0;
    let j = height.length-1;
    while(i < j) {
        if (height[i] < height[j]) {
            height[i] >= leftWall ? (leftWall = height[i]) : (sum += (leftWall-height[i]));
            i++;
        } else {
            height[j] >= rightWall ? (rightWall = height[j]) : (sum += (rightWall-height[j]));
            j--;
        }
    }
    return sum;
    
};
```

#### **[43.Multiply Strings](https://leetcode-cn.com/problems/multiply-strings/)**

Problem：

Given two non-negative integers num1 and num2 represented as strings, return the product of num1 and num2, also represented as a string.

Note: You must not use any built-in BigInteger library or convert the inputs to integer directly.

Example：

```markdown
Input: num1 = "2", num2 = "3"
Output: "6"
```

```js
/**
 * @param {string} num1
 * @param {string} num2
 * @return {string}
 */
var multiply = function(num1, num2) {
    num1 = num1.split("").filter((v)=>v).map((v) => Number(v)).reverse();
    num2 = num2.split("").filter((v)=>v).map((v) => Number(v)).reverse();
    if (!num1.length || !num2.length) {
        return "0";
    }

    if (num1[0] == 0 && num1.length == 1 || num2[0] == 0 && num2.length == 1) {
        return "0";
    }
    let mul = function(arr, n, t) {
        let index = 0;
        
        for (let i=0; i<arr.length; i++) {
            let v = arr[i]*n + index;
            t.push(v%10);
            index = Math.floor(v/10);
        }
        while(index) {
            t.push(index%10);
            index = Math.floor(index/10);
        }
        return t;
    }

    let add = function(a, b) {
        let t = [];
        let flag = 0;
        for (let i=0; i<Math.max(a.length, b.length); i++) {
            flag += ((a[i] || 0) + (b[i] || 0));
            t.push(flag%10);
            flag = Math.floor(flag/10);
        }
        while (flag) {
            t.push(flag%10);
            flag = Math.floor(flag/10);
        }
        return t;
    }
    let total = [];
    let t = [];
    for (let i=0; i<num2.length; i++) {
        total.push(mul(num1, num2[i], [].concat(t)));
        t.push(0);
    }
    let res = [];
    for (let i=0; i<total.length; i++) {
        if (i==0) {
            res = total[i];
        } else {
            res = add(res, total[i]);
        }
    }
    return res.reverse().join("");
};
```

#### **[45.Jump Game II](https://leetcode-cn.com/problems/jump-game-ii/)**

Problem：

Given an array of non-negative integers nums, you are initially positioned at the first index of the array.

Each element in the array represents your maximum jump length at that position.
Your goal is to reach the last index in the minimum number of jumps.

Example：

```markdown
Input: nums = [2,3,1,1,4]
Output: 2
```

Explanation: The minimum number of jumps to reach the last index is 2. Jump 1 step from index 0 to 1, then 3 steps to the last index.

```js
/**
 * @param {number[]} nums
 * @return {number}
 */
var jump = function(nums) {
    if (nums.length <= 1) {
        return 0;
    }
    // 当前所能达到的最大步数和时间
    let max = 0;
    let cnt = 0;
    for (let i=0; i<nums.length;) {
        if (i==0) {
            max = nums[i];
            cnt++;
            i++;
        } else {
            let t = max;
            cnt++;
            while(i <= t && i<nums.length) {
                if (max < nums[i]+i) {
                    max = nums[i]+i;
                }
                i++;
                if (max >= nums.length-1) {
                    return cnt;
                }
            }
        }
        if (max >= nums.length-1) {
            return cnt;
        }
    }
    return cnt;
};
```

#### **[46.Permutations](https://leetcode-cn.com/problems/permutations/)**

Problem：

Given an array nums of distinct integers, return all the possible permutations. You can return the answer in any order.

Example：

```markdown
Input: nums = [1,2,3]
Output: [[1,2,3],[1,3,2],[2,1,3],[2,3,1],[3,1,2],[3,2,1]]
```

```js
/**
 * @param {number[]} nums
 * @return {number[][]}
 */
 var permute = function(nums) {
    let result = [];

    let h = function(index) {
        if (index == nums.length-1) {
            result.push([...nums]);
            return;
        }
        for (let j=index; j<nums.length; j++) {
            let t = nums[index];
            nums[index] = nums[j];
            nums[j] = t;
            h(index+1);
            t = nums[index];
            nums[index] = nums[j];
            nums[j] = t;
        }
    }
    h(0);
    return result;
};
```

#### **[47.Permutations II](https://leetcode-cn.com/problems/permutations-ii/)**

Problem：

Given a collection of numbers, nums, that might contain duplicates, return all possible unique permutations in any order.

Example：

```markdown
Input: nums = [1,1,2]
Output:
[[1,1,2],
 [1,2,1],
 [2,1,1]]
```

```js
/**
 * @param {number[]} nums
 * @return {number[][]}
 */
var permuteUnique = function(nums) {
    let result = [];
    let set_ = new Set();
    let h = function(index) {
        if (index == nums.length-1) {
            if (set_.has(nums.join(""))) {
                return;
            }
            result.push([...nums]);
            set_.add(nums.join(""));
            return;
        }
        for (let j=index; j<nums.length; j++) {
            if (j < nums.length-1 &&  nums[j] == nums[j+1]) {
                continue;
            }
            let t = nums[index];
            nums[index] = nums[j];
            nums[j] = t;
            h(index+1);
            t = nums[index];
            nums[index] = nums[j];
            nums[j] = t;
        }
    }
    h(0);
    return result;
};
```

#### **[48.Rotate Image](https://leetcode-cn.com/problems/rotate-image/)**

Problem：

You are given an n x n 2D matrix representing an image, rotate the image by 90 degrees (clockwise).

You have to rotate the image in-place, which means you have to modify the input 2D matrix directly. DO NOT allocate another 2D matrix and do the rotation.

![mat1](https://github.com/xingwy/Hugging-Algorithm/blob/master/images/mat1.jpg)

Example：

```markdown
Input: matrix = [[1,2,3],[4,5,6],[7,8,9]]
Output: [[7,4,1],[8,5,2],[9,6,3]]
```

```js
/**
 * @param {number[][]} matrix
 * @return {void} Do not return anything, modify matrix in-place instead.
 */
var rotate = function(matrix) {
    let N = matrix.length;
    for (let i=0; i<N/2; i++) {
        for (let j=0; j<N; j++) {
            let tmp = matrix[i][j];
            matrix[i][j] = matrix[N-i-1][j];
            matrix[N-i-1][j] = tmp;
        }
    }
    for (let i=0; i<N; i++) {
        for (let j=0; j<i; j++) {
            let tmp = matrix[i][j];
            matrix[i][j] = matrix[j][i];
            matrix[j][i] = tmp;
        }
    }
};
```

#### **[49.Group Anagrams](https://leetcode-cn.com/problems/group-anagrams/)**

Problem：

Given an array of strings strs, group the anagrams together. You can return the answer in any order.

An Anagram is a word or phrase formed by rearranging the letters of a different word or phrase, typically using all the original letters exactly once.

Example：

```markdown
Input: strs = ["eat","tea","tan","ate","nat","bat"]
Output: [["bat"],["nat","tan"],["ate","eat","tea"]]
```

```js
/**
 * @param {string[]} strs
 * @return {string[][]}
 */
var groupAnagrams = function(strs) {
    let _map = new Map();
    for (let s of strs) {
        let key = s.split("").sort((a, b) => (a>b ? 1 : -1)).join("");
        if (!_map.has(key)) {
            _map.set(key, new Array());
        }
        _map.get(key).push(s);
    }
    let result = [];
    for (let [k, v] of _map) {
        result.push(v);
    }
    return result;
};
```

#### **[50.Pow(x, n)](https://leetcode-cn.com/problems/powx-n/)**

Problem：

Implement [pow(x, n)](http://www.cplusplus.com/reference/valarray/pow/), which calculates `x` raised to the power `n` (i.e., `xn`).

Example：

```markdown
Input: x = 2.00000, n = 10
Output: 1024.00000

Input: x = 2.00000, n = -2
Output: 0.25000
Explanation: 2-2 = 1/22 = 1/4 = 0.25
```

```js
if (n<0) {
        x = 1/x;
        n = -n;
    }
    if (n == 0) {
        return 1;
    }
    let dfs = function(index) {
        if (index == 1) {
            return x;
        } else {
            if (index%2) {
                let v = dfs(index-1)*x;
                return v;
            } else {
                let v = dfs(index/2);
                return v*v;
            }
        }
    }
    return dfs(n);
};
```

#### **[51.N-Queens](https://leetcode-cn.com/problems/n-queens/)**

Problem：The n-queens puzzle is the problem of placing n queens on an n x n chessboard such that no two queens attack each other.

Given an integer n, return all distinct solutions to the n-queens puzzle. You may return the answer in any order.

Each solution contains a distinct board configuration of the n-queens' placement, where 'Q' and '.' both indicate a queen and an empty space, respectively.

![queens_51](https://github.com/xingwy/Hugging-Algorithm/blob/master/images/queens_51.jpg)

Example：

```markdown
Input: n = 4
Output: [[".Q..","...Q","Q...","..Q."],["..Q.","Q...","...Q",".Q.."]]
```

Explanation: There exist two distinct solutions to the 4-queens puzzle as shown above.

```js
/**
 * @param {number} n
 * @return {string[][]}
 */
var solveNQueens = function(n) {
    // 全排列题+剪枝
    let res = [];
    let list = [];
    let use_map_up = new Set();
    let use_map_down = new Set();
    for (let i=0; i<n; i++) {
        list.push(i)
    }
    let base = [];
    for (let i=0; i<n; i++) {
        base.push(".");
    }
    let dfs = function(i, arr) {
        if (i == arr.length-1) {
            // 结束
            if (!use_map_up.has(arr[i]+i-1) && !use_map_down.has(arr[i]-i+1)) {
                res.push([].concat(arr))
            }
        } else {
            // 剪枝
            // 剩余皇后选点
            for (let j=i; j<arr.length; j++) {
                let t = arr[i]; arr[i] = arr[j]; arr[j] = t;
                if (!use_map_up.has(arr[i]+i-1) && !use_map_down.has(arr[i]-i+1)) {
                    
                    use_map_up.add(arr[i]+i-1);
                    use_map_down.add(arr[i]-i+1);
                    dfs(i+1, arr);
                    use_map_up.delete(arr[i]+i-1);
                    use_map_down.delete(arr[i]-i+1);
                }
                t = arr[i]; arr[i] = arr[j]; arr[j] = t;
            }
        }
    }
    dfs(0, list);
    res = res.map((v) => {
        let t = [];
        for (let k=0; k<v.length; k++) {
            let s = [].concat(base);
            s[v[k]] = "Q";
            t.push(s.join(""));
        }
        return t;
    })
    return res;
};
```

#### **[52.N-Queens II](https://leetcode-cn.com/problems/n-queens-ii/)**

Problem：

The n-queens puzzle is the problem of placing n queens on an n x n chessboard such that no two queens attack each other.

Given an integer n, return the number of distinct solutions to the n-queens puzzle.

Example：

![queens_51](https://github.com/xingwy/Hugging-Algorithm/blob/master/images/queens_51.jpg)

```markdown
Input: n = 4
Output: 2
```

Explanation: There are two distinct solutions to the 4-queens puzzle as shown.

```js
/**
 * @param {number} n
 * @return {number}
 */
var totalNQueens = function(n) {
    // 全排列题+剪枝
    let res = 0;
    let list = [];
    let use_map_up = new Set();
    let use_map_down = new Set();
    for (let i=0; i<n; i++) {
        list.push(i)
    }
    let dfs = function(i, arr) {
        if (i == arr.length-1) {
            // 结束
            if (!use_map_up.has(arr[i]+i-1) && !use_map_down.has(arr[i]-i+1)) {
                res++;
            }
        } else {
            // 剪枝
            // 剩余皇后选点
            for (let j=i; j<arr.length; j++) {
                let t = arr[i]; arr[i] = arr[j]; arr[j] = t;
                if (!use_map_up.has(arr[i]+i-1) && !use_map_down.has(arr[i]-i+1)) {
                    
                    use_map_up.add(arr[i]+i-1);
                    use_map_down.add(arr[i]-i+1);
                    dfs(i+1, arr);
                    use_map_up.delete(arr[i]+i-1);
                    use_map_down.delete(arr[i]-i+1);
                }
                t = arr[i]; arr[i] = arr[j]; arr[j] = t;
            }
        }
    }
    dfs(0, list);
    return res;
};
```

#### **[53.Maximum Subarray](https://leetcode-cn.com/problems/maximum-subarray/)**

Problem：

Given an integer array nums, find the contiguous subarray (containing at least one number) which has the largest sum and return its sum.

Example：

```markdown
Input: nums = [-2,1,-3,4,-1,2,1,-5,4]
Output: 6
```

Explanation: [4,-1,2,1] has the largest sum = 6.

```js
/**
 * @param {number[]} nums
 * @return {number}
 */
var maxSubArray = function(nums) {
    if (!nums.length) return 0;
    let tmp = nums[0];
    let max = nums[0];
    for (let i=1; i<nums.length; i++) {
        if (tmp < 0) {
            tmp = nums[i];;
        } else {
            tmp += nums[i];
        }
        max = Math.max(tmp, max);
    }
    return max;
};
```

#### **[54.Spiral Matrix](https://leetcode-cn.com/problems/spiral-matrix/)**

Problem：

Given an `m x n` `matrix`, return *all elements of the* `matrix` *in spiral order*.

Example：

![spiral_54](https://github.com/xingwy/Hugging-Algorithm/blob/master/images/spiral_54.jpg)

```markdown
Input: matrix = [[1,2,3],[4,5,6],[7,8,9]]
Output: [1,2,3,6,9,8,7,4,5]
```

```js
/**
 * @param {number[][]} matrix
 * @return {number[]}
 */
var spiralOrder = function(matrix) {
    if (!matrix.length) {
        return [];
    }
    let dfs = function(m, n, i) {
        let total;
        if (m <=0 || n <= 0) {
            return [];
        }
        if (m == 1 || n == 1) {
            total = m*n;
        } else {
            total = 2*(m+n) - 4;
        }
        if(total < 0) {
            return []
        }
        let res = [];
        let pos_i = i;
        let pos_j = i;
        for (let k=1; k<=total; k++) {
            res.push(matrix[pos_i][pos_j]);
            if (k < m) {
                pos_j++;
            } else if (k>=m && k<(m+n-1)) {
                pos_i++;
            } else if (k>=(m+n-1) && k<(2*m+n-2)) {
                pos_j--;
            } else if (k <total) {
                pos_i--;
            }
        }
        res.push(...dfs(m-2, n-2, i+1));
        return res;
    }
    return dfs(matrix[0].length, matrix.length, 0);
};
```

#### **[55.Jump Game](https://leetcode-cn.com/problems/jump-game/)**

Problem: Given an array of non-negative intergers, you are initially positioned at the first index of the array.

Each element int the array represents your maximum jump length at that position.

Determine if you are able to reach the last index.

Example 1:

```text
Input: [2,3,1,1,4]
Output: true
Explanation: Jump 1 step from index 0 to 1, then 3 steps to the last index
```

Example 2:

```text
Input: [3,2,1,0,4]
Output: false
```

Explanation: You will always arrive at index 3 no matter what.Its maximum jump length is 0,which makes it impossable to reach the last index.

Solutions：

We can Maintain a tmp item named 'max' when it check element form the first index to the last index. Of course, its initialization is 0. when current element puls its index is greater than max, set max equal to this element puls its index. Its time complexity is O(n). My code runtime beats 98.82 % of CPP submissions.

Code:

```c++
class Solution {
public:
    bool canJump(vector<int>& nums) {
        int max = 0, res = false;
        for(int i=0; i < nums.size(); i++){
            if((nums[i] + i) > max && max >= i){
                max = nums[i] + i;
            }
        }
        return max >= (nums.size() - 1);
    }
};
```

#### **[56.Merge Intervals](https://leetcode-cn.com/problems/merge-intervals/)**

Given a collection of intervals, merge all overlapping intervals.

Example 1:

```text
Input: [[1,3],[2,6],[8,10],[15,18]]
Output: [[1,6],[8,10],[15,18]]
```

Explanation: Since intervals [1,3] and [2,6] overlaps, merge them into [1,6].

Example 2:

```text
Input: [[1,4],[4,5]]
Output: [[1,5]]
```

Explanation: Intervals [1,4] and [4,5] are considered overlapping.

Solutions:

My first idea is start at the first element,set type [left, right], judging current element(A) and the previous element(B),merge A and B when A's right is greater than B's left.

Code:

```js
var merge = function(intervals) {
    let res = [], tmp;
    intervals.sort((a,b) => {
        return a[0] - b[0]
    })
    if(intervals.length > 1){
        tmp = intervals[0];
    }else{
        return intervals
    }
    for(let i=1; i<intervals.length; i++){
        if(tmp[1] >= intervals[i][0]){
            tmp[1] = intervals[i][1] > tmp[1] ? intervals[i][1] : tmp[1];
        }else{
            res.push(tmp);
            tmp = intervals[i];
        }
        if(i >= intervals.length-1){
            res.push(tmp);
            break;
        }
    }
    return res; 
};
```

#### **[57.Insert Interval](https://leetcode-cn.com/problems/insert-interval/)**

Given a set of non-overlapping intervals, insert a new interval into the intervals(merge if necessary).

You will assume that the intervals were initially sorted according tp theri start times.

Example 1:

```text
Input: intervals = [[1,3],[6,9]], newInterval = [2,5]
Output: [[1,5],[6,9]]
```

Example 2:

```text
Iput: intervals = [[1,2],[3,5],[6,9],[8,10],[12,16]], newInterval = [4,8]
Output: [[1,2],[3,10],12,16]
```

Explanation: Because the new interval [4,8] overlaps with [3,5],[6,7],[8,10]

Solutions:

We can get the answer of this problem as same as Code problem 56, in face, it is correct.but it isn't the best way solution. because it is non-overlapping, so we need only find the best index and insert newInterval.

Code 1:

```js
var insert = function(intervals, newInterval) {
    let res = [], tmp;
    intervals.push(newInterval)
    intervals.sort((a,b) => {
        return a[0] - b[0]
    })
    if(intervals.length > 1){
        tmp = intervals[0];
    }else{
        return intervals
    }
    for(let i=1; i<intervals.length; i++){
        if(tmp[1] >= intervals[i][0]){
            tmp[1] = intervals[i][1] > tmp[1] ? intervals[i][1] : tmp[1];
        }else{
            res.push(tmp);
            tmp = intervals[i];
        }
        if(i >= intervals.length-1){
            res.push(tmp);
            break;
        }
    }
    return res; 
};
```

#### **[58.Length of Last Word](https://leetcode-cn.com/problems/length-of-last-word/)**

Problem：

Given a string s consists of some words separated by spaces, return the length of the last word in the string. If the last word does not exist, return 0.

A word is a maximal substring consisting of non-space characters only.

Example：

```markdown
Input: s = "Hello World"
Output: 5
```

```js
/**
 * @param {string} s
 * @return {number}
 */
var lengthOfLastWord = function(s) {
    let index = 0;
    for (let i=s.length-1; i>=0; i--) {
        if (s[i] != ' ') {
            index++;
        } else {
            if (index != 0) break;
        }
    }
    return index;
};
```

#### **[59.Spiral Matrix II](https://leetcode-cn.com/problems/spiral-matrix-ii/)**

Problem：

Given a positive integer n, generate an n x n matrix filled with elements from 1 to n2 in spiral order.

Example：

![spiraln_59](https://github.com/xingwy/Hugging-Algorithm/blob/master/images/spiraln_59.jpg)

```markdown
Input: n = 3
Output: [[1,2,3],[8,9,4],[7,6,5]]
```

```js
/**
 * @param {number} n
 * @return {number[][]}
 */
var generateMatrix = function(n) {
    let matrix = [];
    for (let i=0; i<n; i++) {
        matrix.push([]);
    }
    let index = 0;
    let dfs = function(m, n, i) {
        let total;
        if (m <=0 || n <= 0) {
            return [];
        }
        if (m == 1 || n == 1) {
            total = m*n;
        } else {
            total = 2*(m+n) - 4;
        }
        if(total < 0) {
            return []
        }
        let pos_i = i;
        let pos_j = i;
        for (let k=1; k<=total; k++) {
            matrix[pos_i][pos_j] = ++index;
            if (k < m) {
                pos_j++;
            } else if (k>=m && k<(m+n-1)) {
                pos_i++;
            } else if (k>=(m+n-1) && k<(2*m+n-2)) {
                pos_j--;
            } else if (k <total) {
                pos_i--;
            }
        }
        dfs(m-2, n-2, i+1)
    }
    dfs(n, n, 0);
    return matrix;
};
```

#### **[60.Permutation Sequence](https://leetcode-cn.com/problems/permutation-sequence/)**

Problem：

The set [1, 2, 3, ..., n] contains a total of n! unique permutations.

By listing and labeling all of the permutations in order, we get the following sequence for n = 3:

1. `"123"`
2. `"132"`
3. `"213"`
4. `"231"`
5. `"312"`
6. `"321"`

Given `n` and `k`, return the `kth` permutation sequence.

Example：

```markdown
Input: n = 3, k = 3
Output: "213"
```

```js
 var getPermutation = function(n, k) {
    k--;
    let nums = [];
    let acl = function(c) {
        if (c <= 1) {
            return 1;
        }
        return acl(c-1)*c;
    }
    for (let i=1; i<=n; i++) {
        nums.push(i);
    }
    let result = [];
    for (let i=n; i>=1; i--) {
        let flag = acl(i-1);
        let id = Math.floor(k/flag);
        result.push(nums[id]);
        nums[id] = null;
        nums = nums.filter((v) => v);
        k %= flag;
    }
    return result.join("");
};
```













