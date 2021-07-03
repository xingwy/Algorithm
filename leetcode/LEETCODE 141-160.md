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

