### **LEETCODE 181-200**

#### **[187. Repeated DNA Sequences](https://leetcode-cn.com/problems/repeated-dna-sequences/)**

Problem：

The DNA sequence is composed of a series of nucleotides abbreviated as 'A', 'C', 'G', and 'T'.

- For example, "ACGAATTCCG" is a DNA sequence.


When studying DNA, it is useful to identify repeated sequences within the DNA.

Given a string s that represents a DNA sequence, return all the 10-letter-long sequences (substrings) that occur more than once in a DNA molecule. You may return the answer in any order.

Example：

```markdown
Input: s = "AAAAACCCCCAAAAACCCCCCAAAAAGGGTTT"
Output: ["AAAAACCCCC","CCCCCAAAAA"]
```

```js
/**
 * @param {string} s
 * @return {string[]}
 */
var findRepeatedDnaSequences = function(s) {
    let _map = new Map();
    let res = [];
    for (let i=0; i<=s.length-10; i++) {
        let v = s.substring(i,i+10);
        if (!_map.has(v)) {
            _map.set(v, 1);
        } else {
            let t = _map.get(v);
            if (t == 1) {
                res.push(v);
            }
            _map.set(v, t+1) ;
        }
    }
    return res;
};
```




