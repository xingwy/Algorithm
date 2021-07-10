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

#### **[393. UTF-8 Validation](https://leetcode-cn.com/problems/utf-8-validation/)**

Problem：

Given an integer array data representing the data, return whether it is a valid UTF-8 encoding.

A character in UTF8 can be from 1 to 4 bytes long, subjected to the following rules:

For a 1-byte character, the first bit is a 0, followed by its Unicode code.
For an n-bytes character, the first n bits are all one's, the n + 1 bit is 0, followed by n - 1 bytes with the most significant 2 bits being 10.

This is how the UTF-8 encoding would work:

```markdown
  Char. number range  |        UTF-8 octet sequence
      (hexadecimal)    |              (binary)
   --------------------+---------------------------------------------
   0000 0000-0000 007F | 0xxxxxxx
   0000 0080-0000 07FF | 110xxxxx 10xxxxxx
   0000 0800-0000 FFFF | 1110xxxx 10xxxxxx 10xxxxxx
   0001 0000-0010 FFFF | 11110xxx 10xxxxxx 10xxxxxx 10xxxxxx
```

Note: The input is an array of integers. Only the least significant 8 bits of each integer is used to store the data. This means each integer represents only 1 byte of data.

Example：

```markdown
Input: data = [197,130,1]
Output: true
```

Explanation: data represents the octet sequence: 11000101 10000010 00000001.
It is a valid utf-8 encoding for a 2-bytes character followed by a 1-byte character.

```js
/**
 * @param {number[]} data
 * @return {boolean}
 */
var validUtf8 = function(data) {
    let unicode = function(n) {
        if ((0b10000000 & n) == 0b00000000) {
            return 1;
        } else if ((0b11100000 & n) == 0b11000000) {
            return 2;
        } else if ((0b11110000 & n) == 0b11100000) {
            return 3;
        } else if ((0b11111000 & n) == 0b11110000) {
            return 4;
        } else if ((0b11000000 & n) == 0b10000000) {
            return 5;
        } else {
            return 6;
        }
    }
    let index = 0;
    while(index <data.length) {
        let base = unicode(data[index++]);
        if ([1,2,3,4].indexOf(base) != -1) {
            if (data.length - index >= base-1) {
                for (let j=1; j<base; j++) {
                    if (unicode(data[index++]) != 5) {
                        return false;
                    }
                }
            } else {
                return false;
            }
        } else {
            return false;
        }
    }
    return true;
};
```

