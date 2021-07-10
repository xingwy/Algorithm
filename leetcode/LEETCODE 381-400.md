### **LEETCODE 381-400**

#### **[392. Is Subsequence](https://leetcode-cn.com/problems/is-subsequence/)**

Problem：

Given two strings s and t, return true if s is a subsequence of t, or false otherwise.

A subsequence of a string is a new string that is formed from the original string by deleting some (can be none) of the characters without disturbing the relative positions of the remaining characters. (i.e., "ace" is a subsequence of "abcde" while "aec" is not).

Example：

```markdown
Input: s = "abc", t = "ahbgdc"
Output: true
```

Explanation: [4,9] is also accepted.

```js
/**
 * @param {string} s
 * @param {string} t
 * @return {boolean}
 */
var isSubsequence = function(s, t) {
    let index = 0;
    for (let i=0; i<t.length; i++) {
        if (t[i] == s[index]) {
            index++;
        }
        if (index == s.length) {
            return true;
        }
    }
    return index == s.length;
};
```
