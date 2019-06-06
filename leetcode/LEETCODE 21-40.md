### **LEETCODE 21-40**

#### **21. Merge Two Sorted Lists**

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

#### **22. Generate Parentheses**

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



#### **23. Merge K Sorted Lists**

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

#### **24. Swap Nodes in Pairs**

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



#### **25. Reverse Nodes in k-Group**

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

