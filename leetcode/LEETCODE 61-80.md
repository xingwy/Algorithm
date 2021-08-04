### **LEETCODE 61-80**

#### **[61.Rotate List](https://leetcode-cn.com/problems/rotate-list/)**

Problem：Given the `head` of a linked list, rotate the list to the right by `k` places.

Example：

![rotate_list_61](https://github.com/xingwy/Hugging-Algorithm/blob/master/images/rotate_list_61.jpg)

```markdown
Input: head = [1,2,3,4,5], k = 2
Output: [4,5,1,2,3]
```

```js
/**
 * Definition for singly-linked list.
 * function ListNode(val, next) {
 *     this.val = (val===undefined ? 0 : val)
 *     this.next = (next===undefined ? null : next)
 * }
 */
/**
 * @param {ListNode} head
 * @param {number} k
 * @return {ListNode}
 */
var rotateRight = function(head, k) {
    let len = 0;
    let cur = head;
    if (!head) {
        return head;
    }
    while (cur) {
        len++;
        if (!cur.next) {
            if (k%len == 0) {
                return head;
            }
            cur.next = head;
            break;
        }
        cur = cur.next;
    }
    let t = len - (k%len) -1;
    cur = head;
    for (let i=0; i<t; i++) {
        cur = cur.next;
    }
    let res = cur.next;
    cur.next = null;
    return res;
};
```

#### **[62.Unique Paths](https://leetcode-cn.com/problems/unique-paths/)**

Problem：

A robot is located at the top-left corner of a m x n grid (marked 'Start' in the diagram below).

The robot can only move either down or right at any point in time. The robot is trying to reach the bottom-right corner of the grid (marked 'Finish' in the diagram below).

How many possible unique paths are there?

Example：

![robot_maze_62](https://github.com/xingwy/Hugging-Algorithm/blob/master/images/robot_maze_62.png)

```markdown
Input: m = 3, n = 7
Output: 28
```

```js
/**
 * @param {number} m
 * @param {number} n
 * @return {number}
 */
var uniquePaths = function(m, n) {
    let dp = [];
    for (let i=0; i<m; i++) {
        dp[i] = [1];
        if (i == 0) {
            for (let j=1; j<n; j++) {
                dp[i][j] = 1;
            }
        }
    }
    for (let i=1; i<m; i++) {
        for (let j=1; j<n; j++) {
            dp[i][j] = dp[i-1][j] + dp[i][j-1];
        }      
    }
    return dp[m-1][n-1];
};
```

#### **[63.Unique Paths II](https://leetcode-cn.com/problems/unique-paths-ii/)**

Problem：

A robot is located at the top-left corner of a m x n grid (marked 'Start' in the diagram below).

The robot can only move either down or right at any point in time. The robot is trying to reach the bottom-right corner of the grid (marked 'Finish' in the diagram below).

Now consider if some obstacles are added to the grids. How many unique paths would there be?

An obstacle and space is marked as 1 and 0 respectively in the grid.

Example：

![robot_63](https://github.com/xingwy/Hugging-Algorithm/blob/master/images/robot_63.jpg)

```markdown
Input: obstacleGrid = [[0,0,0],[0,1,0],[0,0,0]]
Output: 2
```

Explanation: 

There is one obstacle in the middle of the 3x3 grid above.
There are two ways to reach the bottom-right corner:

1. Right -> Right -> Down -> Down
2. Down -> Down -> Right -> Right

```js
/**
 * @param {number[][]} obstacleGrid
 * @return {number}
 */
var uniquePathsWithObstacles = function(obstacleGrid) {
    let dp = [[]];
    let m = obstacleGrid.length;
    let n = obstacleGrid[0].length;

    if (n>0 && m > 0) {
        if (obstacleGrid[0][0]) {
            dp[0][0] = 0;
        } else {
            dp[0][0] = 1;
        }
    }
    
    for (let i=1; i<m; i++) {
        dp[i] = [];
        dp[i][0] = dp[i-1][0];
        if (obstacleGrid[i][0]) {
            dp[i][0] = 0;
        }
    }

    for (let j=1; j<n; j++) {
        dp[0][j] = dp[0][j-1];
        if (obstacleGrid[0][j]) {
            dp[0][j] = 0;
        }
    }

    for (let i=1; i<m; i++) {
        for (let j=1; j<n; j++) {
            if (obstacleGrid[i][j]) {
                dp[i][j] = 0;
            } else {
                dp[i][j] = dp[i-1][j] + dp[i][j-1];
            }
        }      
    }
    return dp[m-1][n-1];
};
```

#### **[64.Minimum Path Sum](https://leetcode-cn.com/problems/minimum-path-sum/)**

Problem：

Given a m x n grid filled with non-negative numbers, find a path from top left to bottom right, which minimizes the sum of all numbers along its path.

Note: You can only move either down or right at any point in time.

Example：

![minpath_64](https://github.com/xingwy/Hugging-Algorithm/blob/master/images/minpath_64.jpg)

```markdown
Input: grid = [[1,3,1],[1,5,1],[4,2,1]]
Output: 7
```

Explanation: Because the path 1 → 3 → 1 → 1 → 1 minimizes the sum.

```js
/**
 * @param {number[][]} grid
 * @return {number}
 */
var minPathSum = function(grid) {
    let index = 0;
    for (let i=0; i<grid.length; i++) {
        if (i==0) {
            for (let j=1; j<grid[0].length; j++) {
                grid[i][j] = grid[i][j-1] + grid[i][j];
            }
        }else {
            grid[index][0] = grid[i][0] + grid[(index + 1)%2][0];
            for (let j=1; j<grid[i].length; j++) {
                grid[index][j] = Math.min(grid[(index+1)%2][j], grid[index][j-1]) + grid[i][j];
            }
        }
        index = (index + 1) %2;
    }
    return grid[(index + 1) % 2].pop() || 0;
};
```

#### **[65.Valid Number](https://leetcode-cn.com/problems/valid-number/)**

Problem：

A valid number can be split up into these components (in order):

1. A decimal number or an integer.

2. (Optional) An 'e' or 'E', followed by an integer.


A decimal number can be split up into these components (in order):

1. (Optional) A sign character (either '+' or '-').

2. One of the following formats:

   - One or more digits, followed by a dot '.'.

   - One or more digits, followed by a dot '.', followed by one or more digits.

   - A dot '.', followed by one or more digits.

An integer can be split up into these components (in order):

1. (Optional) A sign character (either '+' or '-').
2. One or more digits.

For example, all the following are valid numbers: ["2", "0089", "-0.1", "+3.14", "4.", "-.9", "2e10", "-90E3", "3e+7", "+6e-1", "53.5e93", "-123.456e789"], while the following are not valid numbers: ["abc", "1a", "1e", "e3", "99e2.5", "--6", "-+3", "95a54e53"].

Given a string `s`, return `true` *if* `s` *is a **valid number***.

Example：

```markdown
Input: s = "0"
Output: true
```

```js

```



#### **[66.Plus One](https://leetcode-cn.com/problems/plus-one/)**

Problem：

Given a non-empty array of decimal digits representing a non-negative integer, increment one to the integer.

The digits are stored such that the most significant digit is at the head of the list, and each element in the array contains a single digit.

You may assume the integer does not contain any leading zero, except the number 0 itself.

Example：

```markdown
Input: digits = [1,2,3]
Output: [1,2,4]
```

Explanation: The array represents the integer 123.

```js
/**
 * @param {number[]} digits
 * @return {number[]}
 */
var plusOne = function(digits) {
    let flag = 1;
    for (let i=digits.length-1; i>=0; i--) {
        flag += digits[i];
        if (flag > 9) {
            flag = 1;
            digits[i] = 0;
        } else {
            digits[i] = flag;
            return digits;
        }
    }
    if (flag) {
        digits.splice(0, 0, 1);
    }
    return digits;
};
```

#### **[67.Add Binary](https://leetcode-cn.com/problems/add-binary/)**

Problem：

Given two binary strings `a` and `b`, return *their sum as a binary string*.

Example：

```markdown
Input: a = "11", b = "1"
Output: "100"
```

```js
/**
 * @param {string} a
 * @param {string} b
 * @return {string}
 */
var addBinary = function(a, b) {
    let maxS = a.length >= b.length ? a : b;
    let minS = a.length < b.length ? a : b;
    
    let res = [];
    minL = minS.split("");
    maxL = maxS.split("");
    let base = 0;
    while (maxL.length || base > 0) {
        let v1 = maxL.pop()
        let v2 = minL.pop();
        v1 = v1 ? Number(v1) : 0;
        v2 = v2 ? Number(v2) : 0;

        base = v1 + v2 + base;

        res.push(String(base%2));
        base = Math.floor(base/2);
    }
    return res.reverse().join("");
};
```

#### **[70.Climbing Stairs](https://leetcode-cn.com/problems/climbing-stairs/)**

Problem：

You are climbing a staircase. It takes n steps to reach the top.

Each time you can either climb 1 or 2 steps. In how many distinct ways can you climb to the top?

Example：

```markdown
Input: n = 3
Output: 3
```

Explanation: There are three ways to climb to the top.
1. 1 step + 1 step + 1 step
2. 1 step + 2 steps
3. 2 steps + 1 step

```js
/**
 * @param {number} n
 * @return {number}
 */
let _map = new Map();
var climbStairs = function(n) {
  if (_map.has(n)) {
    return _map.get(n);
  }
  if (n <= 0) {
      return 0;
  }
  if (n <= 3) {
      return n;
  }
  let v = climbStairs(n-1) + climbStairs(n-2);
  _map.set(n, v);
  return v;
};
```

#### [71. Simplify Path](https://leetcode-cn.com/problems/simplify-path/)

Problem：

Given a string path, which is an absolute path (starting with a slash '/') to a file or directory in a Unix-style file system, convert it to the simplified canonical path.

In a Unix-style file system, a period '.' refers to the current directory, a double period '..' refers to the directory up a level, and any multiple consecutive slashes (i.e. '//') are treated as a single slash '/'. For this problem, any other format of periods such as '...' are treated as file/directory names.

The canonical path should have the following format:

- The path starts with a single slash '/'.

- Any two directories are separated by a single slash '/'.

- The path does not end with a trailing '/'.

- The path only contains the directories on the path from the root directory to the target file or directory (i.e., no period '.' or double period '..')

Return the simplified canonical path.

Example：

```markdown
Input: path = "/home//foo/"
Output: "/home/foo"
```

Explanation: In the canonical path, multiple consecutive slashes are replaced by a single one.

```js
/**
 * @param {string} path
 * @return {string}
 */
var simplifyPath = function(path) {
    let list = path.split('/').filter((v) => v);
    let stack = [];
    for (let v of list) {
        if (v == '.') {
            continue;
        } else if (v == '..') {
            stack.pop();
        } else {
            stack.push(v);
        }
    }
    let str = stack.join('/');
    return '/' + str;
};
```

#### [73. Set Matrix Zeroes](https://leetcode-cn.com/problems/set-matrix-zeroes/)

Problem：

Given an m x n integer matrix matrix, if an element is 0, set its entire row and column to 0's, and return the matrix.

You must do it in place.

Example：

![mat2](https://github.com/xingwy/Hugging-Algorithm/blob/master/images/mat2.jpg)

```markdown
Input: matrix = [[0,1,2,0],[3,4,5,2],[1,3,1,5]]
Output: [[0,0,0,0],[0,4,5,0],[0,3,1,0]]
```

```js
/**
 * @param {number[][]} matrix
 * @return {void} Do not return anything, modify matrix in-place instead.
 */
var setZeroes = function(matrix) {
    if (!matrix.length) {
        return;
    }
    let n = matrix.length;
    let m = matrix[0].length;

    let flag = false;
    // 标记头列/头行
    for (let i=0; i<n; i++) {
        if (!matrix[i][0]) {
            flag = true;
        }

        for (let j=1; j<m; j++) {
            if (!matrix[i][j]) {
                matrix[i][0] = matrix[0][j] = 0;
            }
        }
    }
    for (let i=n-1; i>=0; i--) {
        for (let j=1; j<m; j++) {
            if (!matrix[i][0] || !matrix[0][j]) {
                matrix[i][j] = 0;
            }
        }
        if (flag) {
            matrix[i][0] = 0;
        }
    }
};
```

#### [74. Search a 2D Matrix](https://leetcode-cn.com/problems/search-a-2d-matrix/)

Problem：

Write an efficient algorithm that searches for a value in an m x n matrix. This matrix has the following properties:

- Integers in each row are sorted from left to right.
- The first integer of each row is greater than the last integer of the previous row.

Example：

![mat](https://github.com/xingwy/Hugging-Algorithm/blob/master/images/mat.jpg)

```markdown
Input: matrix = [[1,3,5,7],[10,11,16,20],[23,30,34,60]], target = 3
Output: true
```

```js
/**
 * @param {number[][]} matrix
 * @param {number} target
 * @return {boolean}
 */
var searchMatrix = function(matrix, target) {
    let m = matrix.length;
    if (!m) {
        return false;
    }
    let n = matrix[0].length;

    let len = m * n;

    let left = 0;
    let right = len - 1;

    let getValue = function (index) {
        let i = Math.floor(index/n);
        let j = index - i*n;
        return matrix[i][j];
    }

    while (left < right) {
        let mid = Math.floor((left+right)/2);
        let mid_v = getValue(mid);
        
        if (mid_v == target) {
            return true;
        } else if (target < mid_v) {
            right = mid - 1;
        } else {
            left = mid + 1;
        }
    }
    return getValue(left) == target;
};
```

#### [75. Sort Colors](https://leetcode-cn.com/problems/sort-colors/)

Given an array nums with n objects colored red, white, or blue, sort them in-place so that objects of the same color are adjacent, with the colors in the order red, white, and blue.

We will use the integers 0, 1, and 2 to represent the color red, white, and blue, respectively.

You must solve this problem without using the library's sort function.

Example：

```markdown
Input: nums = [2,0,2,1,1,0]
Output: [0,0,1,1,2,2]
```

```js
/**
 * @param {number[]} nums
 * @return {void} Do not return anything, modify nums in-place instead.
 */
var sortColors = function(nums) {
    let left = 0;
    let right = nums.length-1;

    for (let i=0; i<=right; ) {
        if (nums[i] == 0) {
            let t = nums[left];
            nums[left] = nums[i];
            nums[i] = t;
            left++;
            i++;
        } else if (nums[i] == 2) {
            let t = nums[right];
            nums[right] = nums[i];
            nums[i] = t;
            right--;
        } else {
            i++;
        }
    }
};
```

#### [77. Combinations](https://leetcode-cn.com/problems/combinations/)

Problem：

Given two integers n and k, return all possible combinations of k numbers out of the range [1, n].

You may return the answer in any order.

Example：

```markdown
Input: n = 4, k = 2
Output:
[
  [2,4],
  [3,4],
  [2,3],
  [1,2],
  [1,3],
  [1,4],
]
```

```js
/**
 * @param {number} n
 * @param {number} k
 * @return {number[][]}
 */
var combine = function(n, k) {

    let result = [];
    let h = function (l) {
        let index = l.length;
        let lastMin = l[index-1] || 0;
        if (index == k) {
            result.push([...l]);
            return;
        }
        for (let i=lastMin+1; i<=n-k+index+1; i++) {
            l[index] = i;
            if (index == 0) {
            }
            h(l);
            l.pop();
        }
    }
    h([]);
};
```

#### [78. Subsets](https://leetcode-cn.com/problems/subsets/)

Problem：

Given an integer array nums of unique elements, return all possible subsets (the power set).

The solution set must not contain duplicate subsets. Return the solution in any order.

Example：

```markdown
Input: nums = [1,2,3]
Output: [[],[1],[2],[1,2],[3],[1,3],[2,3],[1,2,3]]
```

```js
/**
 * @param {number[]} nums
 * @return {number[][]}
 */
var subsets = function(nums) {
    let result = [];
    let n = nums.length;
    let h = function (k, l) {
        let index = l.length;
        let lastMin = l[index-1] || 0;
        if (index == k) {
            result.push([...l.map(v => nums[v-1])]);
            return;
        }
        for (let i=lastMin+1; i<=n-k+index+1; i++) {
            l[index] = i;
            if (index == 0) {
            }
            h(k, l);
            l.pop();
        }
    }
    for (let k=0; k<=nums.length; k++) {
        h(k, []);
    }
    return result;
};
```



#### **[79.Word Search](https://leetcode-cn.com/problems/word-search/)**

Problem：

Given an m x n grid of characters board and a string word, return true if word exists in the grid.

The word can be constructed from letters of sequentially adjacent cells, where adjacent cells are horizontally or vertically neighboring. The same letter cell may not be used more than once.

Example：

![word2_79](https://github.com/xingwy/Hugging-Algorithm/blob/master/images/word2_79.jpg)

```markdown
Input: board = [
    ["A","B","C","E"],
    ["S","F","C","S"],
    ["A","D","E","E"]
], word = "ABCCED"
Output: true
```

```js
/**
 * @param {character[][]} board
 * @param {string} word
 * @return {boolean}
 */
var exist = function(board, word) {
    
    for (let i=0; i<board.length; i++) {
        for (let j=0; j<board[i].length; j++) {
            let path = new Set();
            let res = dfs(board, i, j, word, 0, path);
            if (res) {
                return true;
            }
        }
    }
    return false;
};

let dfs = function(board, p_i, p_j, word, curIndex, path) {
    if (curIndex >= word.length) {
        return true;
    }
    if (path.has(p_i+"-"+p_j)) {
        return false;
    }
    if (p_i < 0 || p_i >= board.length || p_j < 0 || p_j >= board[0].length) {
        return false;
    }
    let key = word[curIndex];
    if (board[p_i][p_j] != key) {
        return false;
    }

    path.add(p_i+"-"+p_j);
    let r1 = dfs(board, p_i+1, p_j, word, curIndex+1, path);
    if (r1) {
        path.delete(p_i+"-"+p_j);
        return r1;
    }
    
    let r2 = dfs(board, p_i-1, p_j, word, curIndex+1, path);
    if (r2) {
        path.delete(p_i+"-"+p_j);
        return r2;
    }
    let r3 = dfs(board, p_i, p_j+1, word, curIndex+1, path);

    if (r3) {
        path.delete(p_i+"-"+p_j);
        return r3;
    }
    let r4 = dfs(board, p_i, p_j-1, word, curIndex+1, path);

    if (r4) {
        path.delete(p_i+"-"+p_j);
        return r4;
    }
    path.delete(p_i+"-"+p_j);
    return false;
}
```

