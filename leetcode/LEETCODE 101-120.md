### **LEETCODE 101-120**

#### **[104.Maximum Depth of Binary Tree](https://leetcode-cn.com/problems/maximum-depth-of-binary-tree/)**

Problem：

Given the root of a binary tree, return its maximum depth.

A binary tree's maximum depth is the number of nodes along the longest path from the root node down to the farthest leaf node.

Example：

![tmp-tree_104](https://github.com/xingwy/Hugging-Algorithm/blob/master/images/tmp-tree_104.jpg)

```markdown
Input: root = [3,9,20,null,null,15,7]
Output: 3
```

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
var maxDepth = function(root) {
    if (!root) {
        return 0;
    }
    let max = 0;
    let dfs = function(base, index) {
        max = Math.max(index, max);
        base.left && dfs(base.left,index+1);
        base.right && dfs(base.right,index+1);
    }
    dfs(root, 1);
    return max;
};
```

#### **[110.Balanced Binary Tree](https://leetcode-cn.com/problems/balanced-binary-tree/)**

Problem：

Given a binary tree, determine if it is height-balanced.

For this problem, a height-balanced binary tree is defined as:

a binary tree in which the left and right subtrees of every node differ in height by no more than 1.

Example：

![balance_110](https://github.com/xingwy/Hugging-Algorithm/blob/master/images/balance_110.jpg)

```markdown
Input: root = [3,9,20,null,null,15,7]
Output: true
```

```js
/**
 * Definition for a binary tree node.
 * function TreeNode(val, left, right) {
 *     this.val = (val===undefined ? 0 : val)
 *     this.left = (left===undefined ? null : left)
 *     this.right = (right===undefined ? null : right)
 * }
 */
/**
 * @param {TreeNode} root
 * @return {boolean}
 */
var isBalanced = function(root) {
    let res = true;
    let dfs = function(r, t) {
        if (!r) {
            return 0;
        }
        let left = dfs(r.left, t+1);
        let right = dfs(r.right, t+1);
        if (Math.abs(left-right) > 1) {
            res = false
        }
        return Math.max(left, right)+1;
    }
    dfs(root, 0);
    return res;
};
```

#### **[111.Minimum Depth of Binary Tree](https://leetcode-cn.com/problems/minimum-depth-of-binary-tree/)**

Problem：

Given a binary tree, find its minimum depth.

The minimum depth is the number of nodes along the shortest path from the root node down to the nearest leaf node.

Note: A leaf is a node with no children.

Example：

![balance_110](https://github.com/xingwy/Hugging-Algorithm/blob/master/images/balance_110.jpg)

```markdown
Input: root = [3,9,20,null,null,15,7]
Output: 2
```

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
var minDepth = function(root) {
    if (root == null) return 0;
    if (root.left && root.right) {
        return 1 + Math.min(minDepth(root.left), minDepth(root.right));
    } else if (root.left) {
        return 1 + minDepth(root.left);
    } else if (root.right) {
        return 1 + minDepth(root.right);
    } else {
        return 1;
    }
};
```

#### **[112.Path Sum](https://leetcode-cn.com/problems/path-sum/)**

Problem：

Given the root of a binary tree and an integer targetSum, return true if the tree has a root-to-leaf path such that adding up all the values along the path equals targetSum.

A leaf is a node with no children.

Example：

![pathsum_112](https://github.com/xingwy/Hugging-Algorithm/blob/master/images/pathsum_112.jpg)

```markdown
Input: root = [5,4,8,11,null,13,4,7,2,null,null,null,1],
       targetSum = 22
Output: true
```

```js
/**
 * Definition for a binary tree node.
 * function TreeNode(val, left, right) {
 *     this.val = (val===undefined ? 0 : val)
 *     this.left = (left===undefined ? null : left)
 *     this.right = (right===undefined ? null : right)
 * }
 */
/**
 * @param {TreeNode} root
 * @param {number} targetSum
 * @return {boolean}
 */
var hasPathSum = function(root, targetSum) {
    let res = false;
    let dfs = function(node, target) {
        if (res) {
            return;
        }
        if (!node) {
            return;
        }
        target += node.val;
        if (target == targetSum && node.left == null && node.right == null) {
            res = true;
            return;
        }
        dfs(node.left, target);
        dfs(node.right, target);
    }
    dfs(root, 0);
    return res;
};
```

#### **[113.Path Sum II](https://leetcode-cn.com/problems/path-sum-ii/)**

Problem：

Given the root of a binary tree and an integer targetSum, return all root-to-leaf paths where each path's sum equals targetSum.

A leaf is a node with no children.

Example：

![pathsumii_113](https://github.com/xingwy/Hugging-Algorithm/blob/master/images/pathsumii_113.jpg)

```markdown
Input: root = [5,4,8,11,null,13,4,7,2,null,null,5,1],
targetSum = 22
Output: [[5,4,11,2],[5,8,4,5]]
```

```js\
/**
 * Definition for a binary tree node.
 * function TreeNode(val, left, right) {
 *     this.val = (val===undefined ? 0 : val)
 *     this.left = (left===undefined ? null : left)
 *     this.right = (right===undefined ? null : right)
 * }
 */
/**
 * @param {TreeNode} root
 * @param {number} targetSum
 * @return {number[][]}
 */
var pathSum = function(root, targetSum) {
    let res = [];
    let dfs = function(node, target, path) {
        if (!node) {
            return;
        }
        target += node.val;
        let t = [].concat(path)
        t.push(node.val)

        if (target == targetSum && node.left == null && node.right == null) {
            res.push(t);
        }
        dfs(node.left, target, t);
        dfs(node.right, target, t);
    }
    dfs(root, 0, []);
    return res;
};
```



#### **[114.Flatten Binary Tree to Linked List](https://leetcode-cn.com/problems/flatten-binary-tree-to-linked-list/)**

Problem：

Given the root of a binary tree, flatten the tree into a "linked list":

- The "linked list" should use the same TreeNode class where the right child pointer points to the next node in the list and the left child pointer is always null.
- The "linked list" should be in the same order as a pre-order traversal of the binary tree.

Example：

![flaten_114](https://github.com/xingwy/Hugging-Algorithm/blob/master/images/flaten_114.jpg)

```markdown
Input: root = [1,2,5,3,4,null,6]
Output: [1,null,2,null,3,null,4,null,5,null,6]
```

```js
/**
 * Definition for a binary tree node.
 * function TreeNode(val, left, right) {
 *     this.val = (val===undefined ? 0 : val)
 *     this.left = (left===undefined ? null : left)
 *     this.right = (right===undefined ? null : right)
 * }
 */
/**
 * @param {TreeNode} root
 * @return {void} Do not return anything, modify root in-place instead.
 */
var flatten = function(root) {
    let list = [];
    let dps = function(node) {
        if (!node) {
            return null;
        }
        list.push(node);
        dps(node.left);
        dps(node.right);
    }
    dps(root)
    if (!list.length) {
        return null;
    }
    for (let i=0; i<list.length; i++) {
        if (i == list.length-1) {
            list[i].left = null;
            list[i].right = null;
        } else {
            list[i].left = null;
            list[i].right = list[i+1];
        }
    }
};
```

#### **[116.Populating Next Right Pointers in Each Node](https://leetcode-cn.com/problems/populating-next-right-pointers-in-each-node/)**

Problem：

You are given a perfect binary tree where all leaves are on the same level, and every parent has two children. The binary tree has the following definition:

```markdown
struct Node {
  int val;
  Node *left;
  Node *right;
  Node *next;
}
```

Populate each next pointer to point to its next right node. If there is no next right node, the next pointer should be set to NULL.

Initially, all next pointers are set to NULL.

Follow up:

- You may only use constant extra space.
- Recursive approach is fine, you may assume implicit stack space does not count as extra space for this problem.

Example：

![116_sample](https://github.com/xingwy/Hugging-Algorithm/blob/master/images/116_sample.png)

```markdown
Input: root = [1,2,3,4,5,6,7]
Output: [1,#,2,3,#,4,5,6,7,#]
```

Explanation: 

Given the above perfect binary tree (Figure A), your function should populate each next pointer to point to its next right node, just like in Figure B. The serialized output is in level order as connected by the next pointers, with '#' signifying the end of each level.

```js
/**
 * // Definition for a Node.
 * function Node(val, left, right, next) {
 *    this.val = val === undefined ? null : val;
 *    this.left = left === undefined ? null : left;
 *    this.right = right === undefined ? null : right;
 *    this.next = next === undefined ? null : next;
 * };
 */

/**
 * @param {Node} root
 * @return {Node}
 */
var connect = function(root) {
    if (!root) {
        return root;
    }
    if (root.left) {
        root.left.next = root.right;
        if (root.next) {
            root.right.next = root.next.left;
        }
        connect(root.left);
        connect(root.right);
    }
    return root;
};
```

#### **[119.Pascal's Triangle II](https://leetcode-cn.com/problems/pascals-triangle-ii/)**

Problem：

Given an integer rowIndex, return the rowIndexth (0-indexed) row of the Pascal's triangle.

In Pascal's triangle, each number is the sum of the two numbers directly above it as shown.

Example：

```markdown
Input: rowIndex = 3
Output: [1,3,3,1]
```

```js
/**
 * @param {number} rowIndex
 * @return {number[]}
 */
var getRow = function(rowIndex) {
    let res = [];
    for (let i=0; i<=rowIndex; i++) {
        res.push(1);
        let t = res[0];
        for (let j=1; j<res.length-1; j++) {
            let tmp = res[j];
            res[j] = res[j] + t;
            t = tmp;
        }
    }
    return res;
};
```

#### **[120.Triangle](https://leetcode-cn.com/problems/triangle/)**

Problem：

Given a triangle array, return the minimum path sum from top to bottom.

For each step, you may move to an adjacent number of the row below. More formally, if you are on index i on the current row, you may move to either index i or index i + 1 on the next row.

Example：

```markdown
Input: triangle = [[2],[3,4],[6,5,7],[4,1,8,3]]
Output: 11
Explanation: The triangle looks like:
   2
  3 4
 6 5 7
4 1 8 3
```

The minimum path sum from top to bottom is 2 + 3 + 5 + 1 = 11 (underlined above).

```js
/**
 * @param {number[][]} triangle
 * @return {number}
 */
var minimumTotal = function(triangle) {
    for (let i=triangle.length-2; i>=0; i--) {
        for (let j=0; j < triangle[i].length; j++) {
            triangle[i][j] += Math.min(triangle[i+1][j], triangle[i+1][j+1]);
        }
    }
    return triangle[0] && triangle[0][0] || 0;
};
```

