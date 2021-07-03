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

#### **[198. House Robber](https://leetcode-cn.com/problems/house-robber/)**

Problem：

You are a professional robber planning to rob houses along a street. Each house has a certain amount of money stashed, the only constraint stopping you from robbing each of them is that adjacent houses have security systems connected and it will automatically contact the police if two adjacent houses were broken into on the same night.

Given an integer array nums representing the amount of money of each house, return the maximum amount of money you can rob tonight without alerting the police.

Example：

```markdown
Input: nums = [1,2,3,1]
Output: 4
```

Explanation: Rob house 1 (money = 1) and then rob house 3 (money = 3).
Total amount you can rob = 1 + 3 = 4.

```js
/**
 * @param {number[]} nums
 * @return {number}
 */
var rob = function(nums) {
    let db = [];
    function Fn(index) {
        return db[index] || 0;
    }
    for (let i=0; i<nums.length; i++) {
        db[i] = Math.max(nums[i] + Fn(i - 2), Fn(i - 1));
    }
    return db[nums.length - 1] || 0;
};
```

