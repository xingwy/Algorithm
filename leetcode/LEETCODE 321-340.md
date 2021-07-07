### **LEETCODE 321-340**

#### **[329. Longest Increasing Path in a Matrix](https://leetcode-cn.com/problems/longest-increasing-path-in-a-matrix/)**

Problem：

Given an m x n integers matrix, return the length of the longest increasing path in matrix.

From each cell, you can either move in four directions: left, right, up, or down. You may not move diagonally or move outside the boundary (i.e., wrap-around is not allowed).

Example：

![grid1](https://github.com/xingwy/Hugging-Algorithm/blob/master/images/grid1.jpg)

```markdown
Input: matrix = [[9,9,4],[6,6,8],[2,1,1]]
Output: 4
```

Explanation: The longest increasing path is [1, 2, 6, 9].

```js
/**
 * @param {number} A
 * @param {number} B
 * @param {number} C
 * @param {number} D
 * @param {number} E
 * @param {number} F
 * @param {number} G
 * @param {number} H
 * @return {number}
 */
var computeArea = function(A, B, C, D, E, F, G, H) {
    let area = (C-A)*(D-B) + (H-F)*(G-E);
    let minx = Math.max(A, E);
    let miny = Math.max(B, F);
    let maxx = Math.min(C, G);
    let maxy = Math.min(D, H);
    if (minx > maxx || miny > maxy) {
        return area;
    } else {
        return area - (maxy - miny)*(maxx - minx);
    }
};
```
