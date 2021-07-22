### **LEETCODE 821-840**

#### **[828. Count Unique Characters of All Substrings of a Given String](https://leetcode-cn.com/problems/count-unique-characters-of-all-substrings-of-a-given-string/)**

Problem：

Let's define a function countUniqueChars(s) that returns the number of unique characters on s.

- For example if s = "LEETCODE" then "L", "T", "C", "O", "D" are the unique characters since they appear only once in s, therefore countUniqueChars(s) = 5.

Given a string s, return the sum of countUniqueChars(t) where t is a substring of s.

Notice that some substrings can be repeated so in this case you have to count the repeated ones too.

Example：

```markdown
Input: s = "ABC"
Output: 10
```

Explanation: All possible substrings are: "A","B","C","AB","BC" and "ABC".
Evey substring is composed with only unique letters.
Sum of lengths of all substring is 1 + 1 + 1 + 2 + 2 + 3 = 10

```js
/**
 * @param {string} s
 * @return {number}
 */
var uniqueLetterString = function(s) {
    let _map = new Map();
    let cnt = 0;
    for (let i=0; i<s.length; i++) {
        if (!_map.has(s[i])) {
            _map.set(s[i], [-1]);
        }
        _map.get(s[i]).push(i);
    }

    for (let [k, v] of _map) {
        v.push(s.length);
    }

    for (let [k, v] of _map) {
        for (let i=1; i<v.length-1; i++) {
            let prev = v[i-1];
            let tail = v[i+1];
            let curr = v[i];
            cnt += (curr-prev)*(tail-curr);
            cnt %= 1000000007;
        }
    }

    return cnt;
};
```

