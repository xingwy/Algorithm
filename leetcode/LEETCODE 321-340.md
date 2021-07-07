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
 * @param {number[][]} matrix
 * @return {number}
 */
var longestIncreasingPath = function(matrix) {
    let m = matrix.length;
    if (!m) {
        return 0;
    }
    let n = matrix[0].length;
    let posRecord = new Map();
    let passSet = new Set();
    let dfs = function (i, j, parent) {
        if (i<0 || j<0 || i>=m || j>=n){
            return 0;
        }
        if (matrix[i][j]>= parent) {
            return 0;
        }
        if (passSet.has([i,j].toString())) {
            return 0;
        }
        if (posRecord.has([i,j].toString())) {
            return posRecord.get([i,j].toString());
        }
        {
            // 沿用此点
            passSet.add([i,j].toString());
            let v1 = dfs(i+1, j, matrix[i][j]);
            let v2 = dfs(i-1, j, matrix[i][j]);
            let v3 = dfs(i, j+1, matrix[i][j]);
            let v4 = dfs(i, j-1, matrix[i][j]);
            passSet.delete([i,j].toString());
            posRecord.set([i,j].toString(), Math.max(...[v1, v2, v3, v4]) + 1);
            return Math.max(...[v1, v2, v3, v4]) + 1;
        }
    }
    let max = 0;
    for (let i=0; i<matrix.length; i++) {
        for (let j=0; j<matrix[0].length; j++) {
            max = Math.max(max, dfs(i,j,Infinity));
        }
    }
    return max;
};
```

#### **[336. Palindrome Pairs](https://leetcode-cn.com/problems/palindrome-pairs/)**

Problem：

Given a list of **unique** words, return all the pairs of the ***distinct\*** indices `(i, j)` in the given list, so that the concatenation of the two words `words[i] + words[j]` is a palindrome.

Example：

```markdown
Input: words = ["abcd","dcba","lls","s","sssll"]
Output: [[0,1],[1,0],[3,2],[2,4]]
```

Explanation: The palindromes are ["dcbaabcd","abcddcba","slls","llssssll"]

```js
/**
 * 代分解子串
 * @param {*} s
 * @return [] 
 */
function manacher(s) {
    let n = s.length;
    let tmp = '#'+ s.split('').join("*") +'!';
    let m = n * 2;
    let dp = [];
    let ret = [];
    let p = 0, max = -1;
    for (let i = 1; i < m; i++) {
        dp[i] = max >= i ? Math.min(dp[2 * p - i], max - i) : 0;
        while (tmp[i - dp[i] - 1] == tmp[i + dp[i] + 1] && i + dp[i] + 1 < m) {
            dp[i] += 1;
        }
        if (i + dp[i] > max) {
            p = i, max = i + dp[i];
        }
        if (i - dp[i] == 1) {
            // 前缀
            if (ret[Math.floor((i + dp[i]) / 2)] == undefined) {
                ret[Math.floor((i + dp[i]) / 2)] = []
            }
            ret[Math.floor((i + dp[i]) / 2)][0] = true;
        }
        if (i + dp[i] == m - 1) {
            // 后缀
            if (ret[Math.floor((i - dp[i]) / 2)] == undefined) {
                ret[Math.floor((i - dp[i]) / 2)] = []
            }
            ret[Math.floor((i - dp[i]) / 2)][1] = true;
        }
    }
    return ret;
}


var palindromePairs = function(words) {
    let res = [];
    let _map = new Map();
    // 反转元素Map
    for (let i=0; i<words.length; i++) {
        _map.set(words[i].split("").reverse().join(""), i);
    }
    for (let i = 0; i < words.length; i++) {
        let m = words[i].length;
        if (!m) {
            continue;
        }
        let ret = manacher(words[i]);
        let v = _map.get(words[i]);
        if (v != undefined && v != i) {
            res.push([i, v]);
        }
        for (let j = 0; j <= m; j++) {
            if (!ret[j]) {
                continue;
            }
            if (ret[j][0]) {
                // 满足前缀回文
                v = _map.get(words[i].substring(j+1, m));
                if (v != undefined && v != i) {
                    res.push([v, i]);
                }
            }
            if (ret[j][1]) {
                // 满足后缀回文
                v = _map.get(words[i].substring(0, j));
                if (v != undefined && v != i) {
                    res.push([i, v]);
                }
            }
        }
    }
    return res;
};
```

#### **[337. House Robber III](https://leetcode-cn.com/problems/house-robber-iii/)**

problem：

The thief has found himself a new place for his thievery again. There is only one entrance to this area, called root.

Besides the root, each house has one and only one parent house. After a tour, the smart thief realized that all houses in this place form a binary tree. It will automatically contact the police if two directly-linked houses were broken into on the same night.

Given the root of the binary tree, return the maximum amount of money the thief can rob without alerting the police.

Example：

![rob2-tree](https://github.com/xingwy/Hugging-Algorithm/blob/master/images/rob2-tree.jpg)

```markdown
Input: root = [3,4,5,1,3,null,1]
Output: 9
```

Explanation: Maximum amount of money the thief can rob = 4 + 5 = 9.

```js
/**
 * Definition for a binary tree node.
 * function TreeNode(val) {
 *     this.val = val;
 *     this.left = this.right = null;
 * }
 */
/**
 * @param {TreeNode} root
 * @return {number}
 */
var rob = function(root) {
    // pos 当前节点  use 父节点是否采用
    let dfs = function(pos) {
        if (!pos) {
            return [0, 0];
        } else {
            let left_res = dfs(pos.left);
            let right_res = dfs(pos.right);
            let v = Math.max(...left_res) + Math.max(...right_res);
            return [pos.val + left_res[1] + right_res[1], v];
        }
    }
    return Math.max(...dfs(root));
};
```

