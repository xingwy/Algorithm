### **LEETCODE 81-100**

#### **[88.Merge Sorted Array](https://leetcode-cn.com/problems/merge-sorted-array/)**

Problem：You are given two integer arrays nums1 and nums2, sorted in non-decreasing order, and two integers m and n, representing the number of elements in nums1 and nums2 respectively.

Merge nums1 and nums2 into a single array sorted in non-decreasing order.

The final sorted array should not be returned by the function, but instead be stored inside the array nums1. To accommodate this, nums1 has a length of m + n, where the first m elements denote the elements that should be merged, and the last n elements are set to 0 and should be ignored. nums2 has a length of n.

Example：

```markdown
Input: nums1 = [1,2,3,0,0,0], m = 3, nums2 = [2,5,6], n = 3
Output: [1,2,2,3,5,6]
```

Explanation: The arrays we are merging are [1,2,3] and [2,5,6].
The result of the merge is [1,2,2,3,5,6] with the underlined elements coming from nums1.

```js
/**
 * @param {number[]} nums1
 * @param {number} m
 * @param {number[]} nums2
 * @param {number} n
 * @return {void} Do not return anything, modify nums1 in-place instead.
 */
var merge = function(nums1, m, nums2, n) {
    let pos_i = m-1;
    let pos_j = n-1;
    for (let i=m+n-1; i>=0; i--) {
        if (pos_i>=0 && pos_j>=0) {
            if (nums2[pos_j] > nums1[pos_i]) {
                nums1[i] = nums2[pos_j--];
            } else {
                nums1[i] = nums1[pos_i--];
            }
        } else {
            if (pos_i >= 0) {
                break;
            } else {
                nums1[i] = nums2[pos_j--];
            }
        }
    }
};
```

#### **[91.Decode Ways](https://leetcode-cn.com/problems/decode-ways/)**

Problem：

A message containing letters from `A-Z` can be **encoded** into numbers using the following mapping:

```markdown
'A' -> "1"
'B' -> "2"
...
'Z' -> "26"
```

To decode an encoded message, all the digits must be grouped then mapped back into letters using the reverse of the mapping above (there may be multiple ways). For example, "11106" can be mapped into:

"AAJF" with the grouping (1 1 10 6)
"KJF" with the grouping (11 10 6)
Note that the grouping (1 11 06) is invalid because "06" cannot be mapped into 'F' since "6" is different from "06".

Given a string s containing only digits, return the number of ways to decode it.

The answer is guaranteed to fit in a 32-bit integer.

Example：

```markdown
Input: s = "12"
Output: 2
```

Explanation: "12" could be decoded as "AB" (1 2) or "L" (12).

```js
/**
 * @param {string} s
 * @return {number}
 */
var numDecodings = function(s) {
    if (s == "0") {
        return 0;
    }
    let _map = new Map();
    let dfs = function(index) {
        if (_map.has(index)) {
            return _map.get(index);
        }
        if (index >= s.length) {
            return 1;
        }
        let v = Number(s.substring(index,index + 2));
        if ((v >= 11 && v <= 19) || (v>=21 && v<=26)) {
            let t = dfs(index+1) + dfs(index+2);
            if (!_map.has(index)) {
                _map.set(index, t);
            }
            return t;
        } else if (s[index] == "0") {
            _map.set(index, 0);
            return 0;
        } else if ([10,20].includes(v)) {
            let t = dfs(index+2);
            if (!_map.has(index)) {
                _map.set(index, t);
            }
            return t;
        } else {
            let t = dfs(index+1);
            if (!_map.has(index)) {
                _map.set(index, t);
            }
            return t;
        }
    }
    return dfs(0);
};
```

#### **[92.Reverse Linked List II](https://leetcode-cn.com/problems/reverse-linked-list-ii/)**

Problem：

Given the head of a singly linked list and two integers left and right where left <= right, reverse the nodes of the list from position left to position right, and return the reversed list.

Example：

![rev2ex2](https://github.com/xingwy/Hugging-Algorithm/blob/master/images/rev2ex2.jpg)

```markdown
Input: head = [1,2,3,4,5], left = 2, right = 4
Output: [1,4,3,2,5]
```

```js
/**
 * @param {string} s
 * @return {number}
 */
var numDecodings = function(s) {
    if (s == "0") {
        return 0;
    }
    let _map = new Map();
    let dfs = function(index) {
        if (_map.has(index)) {
            return _map.get(index);
        }
        if (index >= s.length) {
            return 1;
        }
        let v = Number(s.substring(index,index + 2));
        if ((v >= 11 && v <= 19) || (v>=21 && v<=26)) {
            let t = dfs(index+1) + dfs(index+2);
            if (!_map.has(index)) {
                _map.set(index, t);
            }
            return t;
        } else if (s[index] == "0") {
            _map.set(index, 0);
            return 0;
        } else if ([10,20].includes(v)) {
            let t = dfs(index+2);
            if (!_map.has(index)) {
                _map.set(index, t);
            }
            return t;
        } else {
            let t = dfs(index+1);
            if (!_map.has(index)) {
                _map.set(index, t);
            }
            return t;
        }
    }
    return dfs(0);
};
```

#### **[93.Restore IP Addresses](https://leetcode-cn.com/problems/restore-ip-addresses/)**

Problem：


Given a string `s` containing only digits, return all possible valid IP addresses that can be obtained from `s`. You can return them in **any** order.

A **valid IP address** consists of exactly four integers, each integer is between `0` and `255`, separated by single dots and cannot have leading zeros. For example, "0.1.2.201" and "192.168.1.1" are **valid** IP addresses and "0.011.255.245", "192.168.1.312" and "192.168@1.1" are **invalid** IP addresses. 

 Example：

```
Input: s = "25525511135"
Output: ["255.255.11.135","255.255.111.35"]
```

```markdown
Input: s = "1111"
Output: ["1.1.1.1"]
```

```js
/**
 * @param {string} s
 * @return {string[]}
 */
var restoreIpAddresses = function(s) {
    let res = [];

    let dfs = function(index, total, list) {
        if (index == 3) {
            if (s[total] == "0" && total != s.length-1 || !s[total]) {
                return;
            }
            let v = s.substring(total);
            if (Number(v)>=0 && Number(v)<=255) {
                list.push(v);
                res.push(list.join("."));
                list.pop();
            }
            return;
        }
        if (s[total] == "0") {
            list.push("0");
            dfs(index+1, total+1, list);
            list.pop();
            return;
        }
        let t = 0;
        while (true) {
            t++;
            let tail = total+t;
            if (tail >= s.length) {
                break;
            }
            let v = s.substring(total, tail);
            if (Number(v)>=0 && Number(v)<=255) {
                list.push(v);
                dfs(index+1, total+t, list);
                list.pop();
            } else {
                break;
            }
            
        }
    }
    dfs(0,0,[]);
    return res;
};
```

#### [94.Binary Tree Inorder Traversal](https://leetcode-cn.com/problems/binary-tree-inorder-traversal/)

Given the root of a binary tree, return the inorder traversal of its nodes' values.

Example：

![inorder_1](https://github.com/xingwy/Hugging-Algorithm/blob/master/images/inorder_1.jpg)

```markdown
Input: root = [1,null,2,3]
Output: [1,3,2]
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
 * @return {number[]}
 */
var inorderTraversal = function(root) {
    let nums = [];
    let arr = [];
    let currentRoot = root;
    while (arr.length > 0 || currentRoot) {
        if (currentRoot) {
        arr.push(currentRoot);
        currentRoot = currentRoot.left;
        } else { 
        let p = arr.pop();
        nums.push(p.val);
        currentRoot = p.right;
        }
    }
    return nums;
};
```

#### **[95.Unique Binary Search Trees II](https://leetcode-cn.com/problems/unique-binary-search-trees-ii/)**

Problem：

Given an integer n, return all the structurally unique BST's (binary search trees), which has exactly n nodes of unique values from 1 to n. Return the answer in any order.

Example：

![uniquebstn3](https://github.com/xingwy/Hugging-Algorithm/blob/master/images/uniquebstn3.jpg)

```markdown
Input: n = 3
Output: [
	[1,null,2,null,3],
	[1,null,3,2],
	[2,1,3],
	[3,1,null,null,2],
	[3,2,null,1]
]
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
 * @param {number} n
 * @return {TreeNode[]}
 */
var generateTrees = function(n) {
    let _map = new Map();
    let dfs = function(left, right) {
        if (left == right) {
            return [new TreeNode(left)];
        }
        if (_map.has(`${left}_${right}`)) {
            return _map.get(`${left}_${right}`)
        }
        let res = [];
        for (let i=left; i<=right; i++) {
            if (i == left) {
                let right_nodes = dfs(i+1, right);
                for (let r of right_nodes) {
                    res.push(new TreeNode(i, undefined, r));
                }
            } else if (i == right) {
                let left_nodes = dfs(left, i-1);
                for (let l of left_nodes) {
                    res.push(new TreeNode(i, l, undefined));
                }
            } else {
                let left_nodes = dfs(left, i-1);
                let right_nodes = dfs(i+1, right);
                for (let l of left_nodes) {
                    for (let r of right_nodes) {
                        res.push(new TreeNode(i, l, r));
                    }
                }
            }
        }
        _map.set(`${left}_${right}`, res);
        return res;
    }

    return dfs(1,n);
};
```

#### **[96.Unique Binary Search Trees](https://leetcode-cn.com/problems/unique-binary-search-trees/)**

Problem：

Given an integer n, return the number of structurally unique BST's (binary search trees) which has exactly n nodes of unique values from 1 to n.

Example：

![uniquebstn3_96](https://github.com/xingwy/Hugging-Algorithm/blob/master/images/uniquebstn3_96.jpg)

```markdown
Input: n = 3
Output: 5
```

```js
/**
 * @param {number} n
 * @return {number}
 */
var numTrees = function(n) {
    let _map = new Map();
    let dfs = function(t) {
        if (t <= 1) {
            return 1;
        } 
        if (_map.has(t)) {
            return _map.get(t);
        }
        let res = 0;
        for (let i=1; i<=t; i++) {
            res += dfs(i-1)*dfs(t-i);
        }
        _map.set(t, res);
        return res;
    }

    return dfs(n);
};
```

#### **[98.Validate Binary Search Tree](https://leetcode-cn.com/problems/validate-binary-search-tree/)**

Problem：

Given the root of a binary tree, determine if it is a valid binary search tree (BST).

A valid BST is defined as follows:

- The left subtree of a node contains only nodes with keys less than the node's key.
- The right subtree of a node contains only nodes with keys greater than the node's key.
- Both the left and right subtrees must also be binary search trees.

Example：

![tree_98](https://github.com/xingwy/Hugging-Algorithm/blob/master/images/tree_98.jpg)

```markdown
Input: root = [5,1,4,null,null,3,6]
Output: false
```

Explanation: The root node's value is 5 but its right child's value is 4.

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
var isValidBST = function(root) {
    let c = function(node, v)  {
        if (v.result) {
            return;
        }
        if (!node) {
            return;
        } else {
            c(node.left, v);
            if (v.val >= node.val) {
                v.result = true;
                return;
            }
            v.val = node.val;
            c(node.right, v);
        }
    }
    let result = {val: -Infinity, result: false};
    c(root, result);
    return !result.result;
};
```

#### **[100.Same Tree](https://leetcode-cn.com/problems/same-tree/)**

Problem：

Given the roots of two binary trees p and q, write a function to check if they are the same or not.

Two binary trees are considered the same if they are structurally identical, and the nodes have the same value.

Example：

![ex1_100](https://github.com/xingwy/Hugging-Algorithm/blob/master/images/ex1_100.jpg)

```markdown
Input: p = [1,2,3], q = [1,2,3]
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
 * @param {TreeNode} p
 * @param {TreeNode} q
 * @return {boolean}
 */
var isSameTree = function(p, q) {
    let r = true;    
    let dfs = function(p, q) {
        if (p == null && q == null) {
            return;
        }
        if (q && p) {
            if (q.val != p.val) {
                r = false;
                return;
            }
            dfs(p.left, q.left);
            dfs(p.right, q.right);
        } else {
            r = false;
            return;
        }
    }
    dfs(p, q);
    return r;
};
```

