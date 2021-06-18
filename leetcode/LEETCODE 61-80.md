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











