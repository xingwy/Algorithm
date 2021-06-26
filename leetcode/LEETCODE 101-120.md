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

