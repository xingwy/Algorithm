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

