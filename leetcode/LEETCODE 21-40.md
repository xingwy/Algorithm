### **LEETCODE 21-40**

#### **[21.Merge Two Sorted Lists](https://leetcode-cn.com/problems/merge-two-sorted-lists/)**

Problem:

```text
Merge two sorted linked lists and return it as a new list. The new List should be made by splicing together the nodes of the first two lists.
```

Example:

```text
Input: 1->2->4, 1->3->4
Output: 1->1->2->3->4->4
```

Solutions:

```text
The linked lists have been sorted, so we can start from the first element and compare the item in order to choose the best item.
```

Code:

```c++
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode(int x) : val(x), next(NULL) {}
 * };
 */
class Solution {
public:
    ListNode* mergeTwoLists(ListNode* l1, ListNode* l2) {
        ListNode *head, *pre;
        head = new ListNode(0);
        pre = head;
        while(l1 || l2){
            if(l1 && l2){
                if(l1->val > l2->val){
                    pre->next = new ListNode(l2->val);
                    l2 = l2->next;
                }else{
                    pre->next = new ListNode(l1->val);
                    l1 = l1->next;
                }
            }else if(l1){
                pre->next = new ListNode(l1->val);
                l1 = l1->next;
            }else{
                pre->next = new ListNode(l2->val);
                l2 = l2->next;
            }
            pre = pre->next;
        }
        return head->next;
    }
};
```

#### **[22.Generate Parentheses](https://leetcode-cn.com/problems/generate-parentheses/)**

Problem:

```text
Given n pairs of parentheses,write a function to generate all combinations of well-formed parentheses.
```

Example:

```text
Input: n = 3
Output:
[
  "((()))",
  "(()())",
  "(())()",
  "()(())",
  "()()()"
]
```

```js
/**
 * @param {number} n
 * @return {string[]}
 */
var generateParenthesis = function(n) {
    let l = 0, r = 0;
    let result = [];
    let dfs = function (l, r, str) {
        if (r > l || l > n) {
            return;
        }
        if (l == n && r == n) {
            result.push(str);
        } else {
            dfs(l+1, r, str + "(");
            dfs(l, r+1, str + ")");
        }
    }
    dfs(0,0,"");
    return result;
};
```



#### **[23. Merge k Sorted Lists](https://leetcode-cn.com/problems/merge-k-sorted-lists/)**

Problem:

```text
Merge k sorted linked lists and return it as one sorted list. Analyze and describe its complexity.
```

Example:

```text
Inpput: [
	1->4->5,
	1->3->4,
	2->6
]
Output: 1->1->2->3->4->4->5->6
```

Solutions:

```text
As same as problem 21, we need choose a best item and insert into the result linked list.
```

Code:

```c++
class Solution {
public:
    ListNode* mergeKLists(vector<ListNode*>& lists) {
	ListNode *head = NULL, *p;
	p = new ListNode(0);
	head = p;
	while (lists.size()) {
		int min = 0x7fffffff;
		int pos_i;
		for (int i = 0; i < lists.size();) {
			
			if (lists[i] != NULL) {
				if (min > lists[i]->val) {
					min = lists[i]->val;
					pos_i = i;
				}
				i++;
			}
			else {
				lists.erase(lists.begin() + i);
			}
		}
		if (lists.size() == 0)
			break;
		p->next = new ListNode(min);
		p = p->next;
		lists[pos_i] = lists[pos_i]->next;
		
	}
	return head->next;
}
    
};
```

#### **[24.Swap Nodes in Pairs](https://leetcode-cn.com/problems/swap-nodes-in-pairs/)**

Problem:

```text
Given a linked list,swap every two adjacent nodes and return its head.
You may not mpdify the values in the list's nodes,only nodes itself may be changed
```

Example:

```text
Given 1->2->3->4, you should return the list as 2->1->4->3
```

Solutions:

```text
This problem's solution is a special method of problem 25's. Set K being equal to 2.
```

Code:

```text
class Solution {
public:
    ListNode* swapPairs(ListNode* head) {
        ListNode *curr = head;
        for(int i=0; i<2; i++){
            if(!curr){
                return head;
            }
            curr = curr->next;
        }
        ListNode* res = reverse(head,curr);
        head->next = swapPairs(curr);
        return res;
    }
    ListNode* reverse(ListNode* head, ListNode* tail){
        ListNode* pre = head;
        while(head != tail) {
            ListNode* tmp = head->next;
            head->next = pre;
            pre = head;
            head = tmp;
        }
        return pre;
    }
};
```



#### **[25.Reverse Nodes in k-Group](https://leetcode-cn.com/problems/reverse-nodes-in-k-group/)**

Problem:

```text
Given a linked list. reverse the nodes of a linked list k at a time and return its modified list.
k is a positive integer and is less than or equal to the length of the length of the linked list. if the number of nodes is not a multiple of k then left-out nodes in the end should remain as it is.
```

Example:

```text
Given this linked list: 1->2->3->4->5
For k=2, you should return: 2->1->4->3->5
For k=3, you should return: 3->2->1->4->5
```

Note:

- Only constant extra memory is allowed.
- You may not alter the values in the list's nodes,only nodes itself may be changed.

Solutions:

```text
Taking a step-bu-step approach, we need make this linked list be smaller linked list with number K, for each small linked list, reverse it.
```

Code:

```c++
class Solution {
public:
    ListNode* reverseKGroup(ListNode* head, int k) {
        ListNode *curr = head;
        for(int i=0; i<k; i++){
            if(!curr){
                return head;
            }
            curr = curr->next;
        }
        ListNode* res = reverse(head,curr);
        head->next = reverseKGroup(curr,k);
        return res;
    }
    ListNode* reverse(ListNode* head, ListNode* tail){
        ListNode *pre = head;
        while (head != tail) {
            ListNode *t = head->next;
            head->next = pre;
            pre = head;
            head = t;
        }
        return pre;
    }
};
```

#### **[26.Remove Duplicates from Sorted Array](https://leetcode-cn.com/problems/remove-duplicates-from-sorted-array/)**

Problem:

```markdown
Given a sorted array nums, remove the duplicates in-place such that each element appears only once and returns the new length.

Do not allocate extra space for another array, you must do this by modifying the input array in-place with O(1) extra memory.
```

Example:

```markdown
Input: nums = [1,1,2]
Output: 2, nums = [1,2]
Explanation: Your function should return length = 2, with the first two elements of nums being 1 and 2 respectively. It doesn't matter what you leave beyond the returned length.
```

```js
/**
 * @param {number[]} nums
 * @return {number}
 */
var removeDuplicates = function(nums) {
    let n = 0;
    for (let i=0; i< nums.length; i++) {
        if(i == 0) {
            n++;
        } else {
            if (nums[n-1] != nums[i]) {
                nums[n] = nums[i];
                n++;
            }
        }
    }
    return n;
};
```

#### **[27.Remove Element](https://leetcode-cn.com/problems/remove-element/)**

Problem:

Given an array nums and a value val, remove all instances of that value in-place and return the new length.

Do not allocate extra space for another array, you must do this by modifying the input array in-place with O(1) extra memory.

The order of elements can be changed. It doesn't matter what you leave beyond the new length.

Note that the input array is passed in by reference, which means a modification to the input array will be known to the caller as well.

```js
/**
 * @param {number[]} nums
 * @param {number} val
 * @return {number}
 */
var removeElement = function(nums, val) {
    let n = 0;
    for (let i=0; i< nums.length; i++) {
        if (val != nums[i]) {
            nums[n] = nums[i];
            n++;
        }
    }
    return n;
};
```

#### **[28.Implement strStr()](https://leetcode-cn.com/problems/implement-strstr/)**

Problems:

```markdown
Implement strStr().

Return the index of the first occurrence of needle in haystack, or -1 if needle is not part of haystack.
```

Example:

```markdown
Input: haystack = "hello", needle = "ll"
Output: 2
```

```js
/**
 * @param {string} haystack
 * @param {string} needle
 * @return {number}
 */
var strStr = function(haystack, needle) {
    if (!needle){
        return 0;
    }
    let t = [];
    for (let i=0; i<needle.length; i++) {
        if (i == 0) {
            t.push(0);
        } else {
            let j = t[i-1];
            while (needle[j] != needle[i] && j > 0) {
                j = t[j-1];
            }
            if (needle[j] == needle[i]) {
                t[i] = j + 1;
            } else {
                t[i] = 0;
            }
        }
    }

    let j = 0;
    for (let i=0; i<haystack.length;) {
        if (needle[j] == haystack[i]) {
            j++;
            i++;
            if (j == needle.length) {
                return i-j;
            }
        } else {
            if (j == 0) {
                i++;
            } else {
                j = t[j-1];
            }
        }
    }
    return -1;
};
```

#### **[29.Divide Two Integers](https://leetcode-cn.com/problems/divide-two-integers/)**

Problems:

```markdown
Given two integers dividend and divisor, divide two integers without using multiplication, division, and mod operator.

Return the quotient after dividing dividend by divisor.

The integer division should truncate toward zero, which means losing its fractional part. For example, truncate(8.345) = 8 and truncate(-2.7335) = -2.

Note: Assume we are dealing with an environment that could only store integers within the 32-bit signed integer range: [−231, 231 − 1]. For this problem, assume that your function returns 231 − 1 when the division result overflows.
```

Example:

```markdown
Input: dividend = 10, divisor = 3
Output: 3
Explanation: 10/3 = truncate(3.33333..) = 3.
```

```js
/**
 * @param {number} dividend
 * @param {number} divisor
 * @return {number}
 */
var divide = function(dividend, divisor) {
    let num = true;
    if (dividend < 0) {
        num = !num;
        dividend = -dividend;
    }
    if (divisor < 0) {
        num = !num;
        divisor = -divisor;
    }
    
    let cnt = 0;
    let quene = [divisor];
    let cnts = [1];
    let index=0;
    while (index>=0 && dividend >= quene[0]) {
        if (dividend >= quene[index]+quene[index] && index<29) {
            quene[index+1] = quene[index] + quene[index];
            cnts[index+1] = cnts[index] + cnts[index];
            index++;
        } else if (dividend < quene[index]) {
            index--;
        } else {
            dividend -= quene[index];
            cnt += cnts[index];
        }
    }
    
    if (!num) {
        cnt = -cnt; 
    }
    if (cnt > 0x7fffffff) {
        cnt = 0x7fffffff;
    }

    if (cnt < -0x80000000) {
        cnt = -0x80000000;
    }
    return cnt;
};
```

#### **[30.Substring with Concatenation of All Words](https://leetcode-cn.com/problems/substring-with-concatenation-of-all-words/)**

Problem：You are given a string s and an array of strings words of the same length. Return all starting indices of substring(s) in s that is a concatenation of each word in words exactly once, in any order, and without any intervening characters.

You can return the answer in any order.

Example：

```markdown
Input: s = "barfoothefoobarman", words = ["foo","bar"]
Output: [0,9]
Explanation: Substrings starting at index 0 and 9 are "barfoo" and "foobar" respectively.
The output order does not matter, returning [9,0] is fine too.
```

```js
/**
 * @param {string} s
 * @param {string[]} words
 * @return {number[]}
 */

// todo 代码可以简化,不过思路大致不变
var findSubstring = function(s, words) {
    let wordSize = words[0].length;
    let wordCount = words.length;
    let result = [];
   
    const getList = function (i, s, d) {
        let list = [];
        let str = '';
        for (let k=i; k<s.length; k++) {
            str += s[k];
            if (str.length >= d) {
                list.push(str);
                str = '';
            }
        }
        return list;
    }
    for (let i=0; i<wordSize; i++) {
        let list = getList(i, s, wordSize);
        let start = 0;
        let end = 0;
        let _mapWord = new Map();
        for (let word of words) {
            let count = _mapWord.get(word) || 0;
            _mapWord.set(word, count + 1);
        }
        while (start <list.length && end < list.length) {
            if (end == 7) {
                console.log(_mapWord)
            }
            let v = _mapWord.get(list[end]);
            // 没有该值
            if (v == null) {
                // 还原_mapWord
                for (let k=start; k<end; k++) {
                    let count = _mapWord.get(list[k]);
                    _mapWord.set(list[k], count + 1);
                }
                end++;
                start = end;
            } else {
                // 未剩余该值
                if (v == 0) {
                    if (list[start] == list[end]) {
                        // 后移
                        start++;
                        end++;
                    } else {
                        // 还原_mapWord
                        let count = _mapWord.get(list[start]);
                        _mapWord.set(list[start], count + 1);
                        start += 1;
                    }
                } else {
                    // 还有剩余
                    _mapWord.set(list[end], v-1);
                    end++;
                }
            }
            if (end - start == wordCount) {
                // 满足匹配
                for (let k=start; k<end; k++) {
                    let count = _mapWord.get(list[k]);
                    _mapWord.set(list[k], count + 1);
                }
                result.push(i + start*wordSize);
                start += 1;
                end = start;
            }
        }
    }
    return result;
};
```

#### **[31.Next Permutation](https://leetcode-cn.com/problems/next-permutation/)**

Problem：

```markdown
Implement next permutation, which rearranges numbers into the lexicographically next greater permutation of numbers.

If such an arrangement is not possible, it must rearrange it as the lowest possible order (i.e., sorted in ascending order).

The replacement must be in place and use only constant extra memory.
```

Example：

```markdown
Input: nums = [5,6,4,3,2,1]
Output: [6,1,2,3,4,5]
```

```js
/**
 * @param {number[]} nums
 * @return {void} Do not return anything, modify nums in-place instead.
 */
var nextPermutation = function(nums) {
    if (nums.length <= 1) {
        return nums;
    }
    let index = 0;
    for (let i=nums.length-1; i>=1; i--) {
        if (nums[i] > nums[i-1]) {
            for (let j = nums.length-1; j >= i; j--) {
                if (nums[j] > nums[i-1]) {
                    let t = nums[j];
                    nums[j] = nums[i-1];
                    nums[i-1] = t;
                    break;
                }
            }
            index = i;
            break;
        }
    }
    let start = index;
    let end = nums.length-1;
    while (start < end) {
        let t = nums[start];
        nums[start] = nums[end];
        nums[end] = t;
        start++;
        end--;
    }
};
```

#### **[32.Longest Valid Parentheses](https://leetcode-cn.com/problems/longest-valid-parentheses/)**

Problem：

```markdown
Given a string containing just the characters '(' and ')', find the length of the longest valid (well-formed) parentheses substring.
```

Example：

```markdown
Input: s = "(()"
Output: 2
Explanation: The longest valid parentheses substring is "()".
```

```js
/**
 * @param {string} s
 * @return {number}
 */
var longestValidParentheses = function(s) {
    // dp[i][j] 表示 i~j 是否为完整有效括号
    let max = 0;
    
    let stack = [-1];
    for (let i=0; i<s.length; i++) {
        if (s[i] == "(") {
            stack.push(i);
        } else {
            stack.pop();
            if (stack.length) {
                max = Math.max(max, i-stack[stack.length-1]);
            } else {
                stack.push(i);
            }
        }
    }
    return max;
};
```

#### **[33.Search in Rotated Sorted Array](https://leetcode-cn.com/problems/search-in-rotated-sorted-array/)**

Problem：

```markdown
There is an integer array nums sorted in ascending order (with distinct values).

Prior to being passed to your function, nums is rotated at an unknown pivot index k (0 <= k < nums.length) such that the resulting array is [nums[k], nums[k+1], ..., nums[n-1], nums[0], nums[1], ..., nums[k-1]] (0-indexed). For example, [0,1,2,4,5,6,7] might be rotated at pivot index 3 and become [4,5,6,7,0,1,2].

Given the array nums after the rotation and an integer target, return the index of target if it is in nums, or -1 if it is not in nums.

You must write an algorithm with O(log n) runtime complexity.
```

Example：

```markdown
Input: nums = [4,5,6,7,0,1,2], target = 0
Output: 4
```

```js
/**
 * @param {number[]} nums
 * @param {number} target
 * @return {number}
 */
var search = function(nums, target) {
  	
};
```

