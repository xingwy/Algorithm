### **LCP 01-10**

#### [剑指 Offer 06. 从尾到头打印链表](https://leetcode-cn.com/problems/cong-wei-dao-tou-da-yin-lian-biao-lcof/)

题：输入一个链表的头节点，从尾到头反过来返回每个节点的值（用数组返回）。

示例：

```markdown
输入：head = [1,3,2]
输出：[2,3,1]
```

解释：小A 每次都猜对了。

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
 * @return {number[]}
 */
var reversePrint = function(head) {
    let result = [];
    let dps = function(node) {
        if (!node) {
            return;
        }
        if (node.next) {
            dps(node.next);
        }
        result.push(node.val);
    }
    dps(head);
    return result;
};
```
