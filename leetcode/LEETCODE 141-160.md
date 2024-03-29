### **LEETCODE 141-160**

#### **[141. Linked List Cycle](https://leetcode-cn.com/problems/linked-list-cycle/)**

Problem：

Given head, the head of a linked list, determine if the linked list has a cycle in it.

There is a cycle in a linked list if there is some node in the list that can be reached again by continuously following the next pointer. Internally, pos is used to denote the index of the node that tail's next pointer is connected to. Note that pos is not passed as a parameter.

Return true if there is a cycle in the linked list. Otherwise, return false.

Example：

![circularlinkedlist](https://github.com/xingwy/Hugging-Algorithm/blob/master/images/circularlinkedlist.png)

```markdown
Input: head = [3,2,0,-4], pos = 1
Output: true
```

Explanation: There is a cycle in the linked list, where the tail connects to the 1st node (0-indexed).

```js
/**
 * Definition for singly-linked list.
 * function ListNode(val) {
 *     this.val = val;
 *     this.next = null;
 * }
 */

/**
 * @param {ListNode} head
 * @return {boolean}
 */
var hasCycle = function(head) {
    let first = head;
    let second = head;
    if (!head) {
        return false
    }
    do {
        first = first.next;
        second = second.next;
        if (!second) {
            return false;
        }
        second = second.next;
        if (!first || !second) {
            return false;
        }
        if (first === second) {
            return true;
        }
    }
    while(first && second);
    return false;
};
```

#### **[142. Linked List Cycle II](https://leetcode-cn.com/problems/linked-list-cycle-ii/)**

Problem：

Given a linked list, return the node where the cycle begins. If there is no cycle, return null.

There is a cycle in a linked list if there is some node in the list that can be reached again by continuously following the next pointer. Internally, pos is used to denote the index of the node that tail's next pointer is connected to. Note that pos is not passed as a parameter.

Notice that you should not modify the linked list.

Example：

![circularlinkedlist](https://github.com/xingwy/Hugging-Algorithm/blob/master/images/circularlinkedlist.png)

```markdown
Input: head = [3,2,0,-4], pos = 1
Output: tail connects to node index 1
```

Explanation: There is a cycle in the linked list, where tail connects to the second node.

```js
/**
 * Definition for singly-linked list.
 * function ListNode(val) {
 *     this.val = val;
 *     this.next = null;
 * }
 */

/**
 * @param {ListNode} head
 * @return {ListNode}
 */
var detectCycle = function(head) {
    let first = head;
    let second = head;
    if (!head) {
        return null
    }
    let round = 0;
    let index = 0;
    do {
        first = first.next;
        second = second.next;
        if (!second) {
            return null;
        }
        second = second.next;
        if (!first || !second) {
            return null;
        }
        if (first === second) {
            // 寻找圆环的环长
            do {
                first = first.next;
                second = second.next.next;
                round++;
            }
            while(first !== second);
            let cur = head;
            let next = head;
            for (let i=0; i<round; i++) {
                next = next.next;
            }
            while(next != cur) {
                cur = cur.next;
                next = next.next;
            }
            return cur;
        }
    }
    while(first && second);
    return null;
};
```

#### **[143. Reorder List](https://leetcode-cn.com/problems/reorder-list/)**

Problem：

You are given the head of a singly linked-list. The list can be represented as:

```markdown
L0 → L1 → … → Ln - 1 → Ln
```

*Reorder the list to be on the following form:*

```markdown
L0 → Ln → L1 → Ln - 1 → L2 → Ln - 2 → …
```

You may not modify the values in the list's nodes. Only nodes themselves may be changed.

Example：

![reorder2-linked-list](https://github.com/xingwy/Hugging-Algorithm/blob/master/images/reorder2-linked-list.jpg)

```markdown
Input: head = [1,2,3,4,5]
Output: [1,5,2,4,3]
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
 * @return {void} Do not return anything, modify head in-place instead.
 */
var reorderList = function(head) {
    let list = []
    let point = head;
    while(point) {
        list.push(point);
        point = point.next;
    }
    let res = new ListNode();
    let prev = res;
    let begin = 0; end = list.length-1;
    while (end >= begin) {
        prev.next = list[begin];
        prev = prev.next;
        begin++;
        if (begin > end) {
            break;
        }
        prev.next = list[end];
        prev = prev.next;
        end--;
    }
    prev.next = null;
    return res.next;
};
```

#### **[144. Binary Tree Preorder Traversal](https://leetcode-cn.com/problems/binary-tree-preorder-traversal/)**

Problem：

Given the root of a binary tree, return the preorder traversal of its nodes' values.

Example：

![inorder_1](https://github.com/xingwy/Hugging-Algorithm/blob/master/images/inorder_1.jpg)

```markdown
Input: root = [1,null,2,3]
Output: [1,2,3]
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
 * @return {number[]}
 */
var preorderTraversal = function(root) {
    let list = [];
    let dfs = function(node) {
        if (!node) {
            return;
        }
        list.push(node.val);
        dfs(node.left);
        dfs(node.right);
    }
    dfs(root);
    return list;
};
```

#### **[145. Binary Tree Postorder Traversal](https://leetcode-cn.com/problems/binary-tree-postorder-traversal/)**

Problem：

Given the root of a binary tree, return the postorder traversal of its nodes' values.

Example：

![pre1](https://github.com/xingwy/Hugging-Algorithm/blob/master/images/pre1.jpg)

```markdown
Input: root = [1,null,2,3]
Output: [3,2,1]
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
 * @return {number[]}
 */
var postorderTraversal = function(root) {
    let list = [];

    let dfs = function(node) {
        if (!node) {
            return;
        }
        dfs(node.left);
        dfs(node.right);
        list.push(node.val);
    }
    dfs(root);
    return list;
};
```

#### **[148. Sort List](https://leetcode-cn.com/problems/sort-list/)**

Problem：

Given the head of a linked list, return the list after sorting it in ascending order.

Follow up: Can you sort the linked list in O(n logn) time and O(1) memory (i.e. constant space)?

Example：

![sort_list_2](https://github.com/xingwy/Hugging-Algorithm/blob/master/images/sort_list_2.jpg)

```markdown
Input: head = [-1,5,3,4,0]
Output: [-1,0,3,4,5]
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
 * @return {ListNode}
 */
var sortList = function(head) {
    let list = []
    let point = head;
    while(point) {
        list.push(point);
        point = point.next;
    }
    let res = new ListNode();
    let prev = res;
    list.sort((a, b) => a.val-b.val);
    for (let i=0; i<list.length; i++) {
        prev.next = list[i];
        prev = prev.next;
    }
    prev.next = null;
    return res.next;
};
```

#### [149. Max Points on a Line](https://leetcode-cn.com/problems/max-points-on-a-line/)

Problem：

Given an array of points where points[i] = [xi, yi] represents a point on the X-Y plane, return the maximum number of points that lie on the same straight line.

Example：

![plane1](https://github.com/xingwy/Hugging-Algorithm/blob/master/images/plane1.jpg)

```markdown
Input: points = [[1,1],[2,2],[3,3]]
Output: 3
```

```js
/**
 * @param {number[][]} points
 * @return {number}
 */
var maxPoints = function(points) {
    // ax + by + c = 0;  确定a b为自然数 a,b保持最大公约数为1 
    let _map = new Map();
    
    let gcd = function (a, b) {
        while (b != 0) {
            let c = a % b;
            a = b;
            b = c;
        }
        return a;
    }
    let max = 1;
    let cal = function (pointA, pointB) {
        let x1 = pointA[0];
        let y1 = pointA[1];
        let x2 = pointB[0];
        let y2 = pointB[1];

        let diffy = y2 - y1;
        let diffx = x2 - x1;
        let key = "";
        if (diffx == 0) {
            key = `x_${x1}`;
            
        } else if (diffy == 0) {
            key = `y_${y1}`;
        } else {
            let g = gcd(diffx, diffy);
            diffx /= g;
            diffy /= g;
            let c = diffx*y1 - diffy*x1;
            key = `${diffy}_${diffx}_${c}`;
            
        }
        if (!_map.has(key)) {
            _map.set(key, new Set());
        }
        _map.get(key).add(x1+"_"+y1);
        _map.get(key).add(x2+"_"+y2);
        max = Math.max(max, _map.get(key).size);
    }

    for (let i=0; i<points.length; i++) {
        for (let j=0; j<points.length; j++) {
            if (i == j) {
                continue;
            }
            cal(points[i], points[j]);
        }
    }
    return max;
};
```

#### **[150. Evaluate Reverse Polish Notation](https://leetcode-cn.com/problems/evaluate-reverse-polish-notation/)**

Problem：

Evaluate the value of an arithmetic expression in Reverse Polish Notation.

Valid operators are +, -, *, and /. Each operand may be an integer or another expression.

Note that division between two integers should truncate toward zero.

It is guaranteed that the given RPN expression is always valid. That means the expression would always evaluate to a result, and there will not be any division by zero operation.

Example：

```markdown
Input: tokens = ["10","6","9","3","+","-11","*","/","*","17","+","5","+"]
Output: 22

```

Explanation: ((10 * (6 / ((9 + 3) * -11))) + 17) + 5
= ((10 * (6 / (12 * -11))) + 17) + 5
= ((10 * (6 / -132)) + 17) + 5
= ((10 * 0) + 17) + 5
= (0 + 17) + 5
= 17 + 5
= 22

```js
/**
 * @param {string[]} tokens
 * @return {number}
 */
var evalRPN = function(tokens) {
    let stack = [];
    while(tokens.length) {
        let c = tokens.shift();
        if (c == "+") {
            let first = stack.pop();
            let second = stack.pop();
            stack.push(second + first);
        } else if (c == "-") {
            let first = stack.pop();
            let second = stack.pop();
            stack.push(second - first);
        } else if (c == "*") {
            let first = stack.pop();
            let second = stack.pop();
            stack.push(second * first);

        } else if (c == "/") {
            let first = stack.pop();
            let second = stack.pop();
            stack.push(parseInt(second / first));

        } else {
            stack.push(Number(c));
        }
    }
    return stack.pop()
};
```

#### **[152. Maximum Product Subarray](https://leetcode-cn.com/problems/maximum-product-subarray/)**

Problem：

Given an integer array nums, find a contiguous non-empty subarray within the array that has the largest product, and return the product.

It is guaranteed that the answer will fit in a 32-bit integer.

A subarray is a contiguous subsequence of the array.

Example：

```markdown
Input: nums = [-2,0,-1]
Output: 0
```

Explanation: The result cannot be 2, because [-2,-1] is not a subarray.

```js
/**
 * @param {number[]} nums
 * @return {number}
 */
var maxProduct = function(nums) {
    let max;
    let max_old, min_old
    max = max_old = min_old = nums[0];
    for (let i=1; i<nums.length; i++) {
        let tmp = Math.max(max_old*nums[i], min_old*nums[i], nums[i]);
        min_old = Math.min(max_old*nums[i], min_old*nums[i], nums[i]);
        max_old = tmp;
        if (max < max_old) {
            max = max_old;
        }
    }
    return max;
};
```

#### **[153. Find Minimum in Rotated Sorted Array](https://leetcode-cn.com/problems/find-minimum-in-rotated-sorted-array/)**

Problem：

Suppose an array of length n sorted in ascending order is rotated between 1 and n times. For example, the array nums = [0,1,2,4,5,6,7] might become:

- [4,5,6,7,0,1,2] if it was rotated 4 times.
- [0,1,2,4,5,6,7] if it was rotated 7 times.

Notice that rotating an array [a[0], a[1], a[2], ..., a[n-1]] 1 time results in the array [a[n-1], a[0], a[1], a[2], ..., a[n-2]].

Given the sorted rotated array nums of unique elements, return the minimum element of this array.

You must write an algorithm that runs in O(log n) time.

Example：

```markdown
Input: nums = [3,4,5,1,2]
Output: 1
```

Explanation: The original array was [1,2,3,4,5] rotated 3 times.\

```js
/**
 * @param {number[]} nums
 * @return {number}
 */
var findMin = function(nums) {
    let mina = nums[0];
    for (let i=1; i<nums.length; i++) {
        if (mina > nums[i]) {
            mina = nums[i];
        }
    }
    return mina;
};
```

#### **[154. Find Minimum in Rotated Sorted Array II](https://leetcode-cn.com/problems/find-minimum-in-rotated-sorted-array-ii/)**

Problem：

Suppose an array of length n sorted in ascending order is rotated between 1 and n times. For example, the array nums = [0,1,4,4,5,6,7] might become:

- [4,5,6,7,0,1,4] if it was rotated 4 times.
- [0,1,4,4,5,6,7] if it was rotated 7 times.

Notice that rotating an array [a[0], a[1], a[2], ..., a[n-1]] 1 time results in the array [a[n-1], a[0], a[1], a[2], ..., a[n-2]].

Given the sorted rotated array nums that may contain duplicates, return the minimum element of this array.

You must decrease the overall operation steps as much as possible.

```markdown
Input: nums = [2,2,2,0,1]
Output: 0
```

```js
/**
 * @param {number[]} nums
 * @return {number}
 */
var findMin = function(nums) {
       let left = 0;
    let right = nums.length-1;
    while (left < right) {
        
        let mid = Math.floor((left + right)/2);
        if (nums[mid] < nums[right]) {
            right = mid;
        } else if (nums[mid] > nums[right]) {
            left = mid+1;
        } else {
            right--;
        }
    }
    
    return nums[right];
};
```



