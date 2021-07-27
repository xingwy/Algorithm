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

#### [剑指 Offer 07. 重建二叉树](https://leetcode-cn.com/problems/zhong-jian-er-cha-shu-lcof/)

题：输入某二叉树的前序遍历和中序遍历的结果，请构建该二叉树并返回其根节点。

假设输入的前序遍历和中序遍历的结果中都不含重复的数字。

示例：

![tree](C:\Users\Administrator\Desktop\Algorithm\images\tree.jpg)

```markdown
Input: preorder = [3,9,20,15,7], inorder = [9,3,15,20,7]
Output: [3,9,20,null,null,15,7]
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
 * @param {number[]} preorder
 * @param {number[]} inorder
 * @return {TreeNode}
 */
var buildTree = function(preorder, inorder) {
    const rebuild = function(pstart, pend, istart, iend) {
        if (pend < pstart || iend < istart) {
            return null;
        };
        let node = new TreeNode(preorder[pstart]);
        for (let i=istart; i<=iend; i++) {
            if(preorder[pstart] == inorder[i]) {
                node.left = rebuild(pstart+1, pstart+(i-istart), istart, i-1);
                node.right = rebuild(pstart+(i-istart)+1, pend, i+1, iend);
            }
        }
        return node;
    }
    return rebuild(0, preorder.length-1, 0, inorder.length-1);
};
```

