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

