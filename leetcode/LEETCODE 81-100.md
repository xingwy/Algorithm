### **LEETCODE 81-100**

#### **[88.Merge Sorted Array](https://leetcode-cn.com/problems/merge-sorted-array/)**

Problem：You are given two integer arrays nums1 and nums2, sorted in non-decreasing order, and two integers m and n, representing the number of elements in nums1 and nums2 respectively.

Merge nums1 and nums2 into a single array sorted in non-decreasing order.

The final sorted array should not be returned by the function, but instead be stored inside the array nums1. To accommodate this, nums1 has a length of m + n, where the first m elements denote the elements that should be merged, and the last n elements are set to 0 and should be ignored. nums2 has a length of n.

Example：

```markdown
Input: nums1 = [1,2,3,0,0,0], m = 3, nums2 = [2,5,6], n = 3
Output: [1,2,2,3,5,6]
```

Explanation: The arrays we are merging are [1,2,3] and [2,5,6].
The result of the merge is [1,2,2,3,5,6] with the underlined elements coming from nums1.

```js
/**
 * @param {number[]} nums1
 * @param {number} m
 * @param {number[]} nums2
 * @param {number} n
 * @return {void} Do not return anything, modify nums1 in-place instead.
 */
var merge = function(nums1, m, nums2, n) {
    let pos_i = m-1;
    let pos_j = n-1;
    for (let i=m+n-1; i>=0; i--) {
        if (pos_i>=0 && pos_j>=0) {
            if (nums2[pos_j] > nums1[pos_i]) {
                nums1[i] = nums2[pos_j--];
            } else {
                nums1[i] = nums1[pos_i--];
            }
        } else {
            if (pos_i >= 0) {
                break;
            } else {
                nums1[i] = nums2[pos_j--];
            }
        }
    }
};
```

#### **[91.Decode Ways](https://leetcode-cn.com/problems/decode-ways/)**

Problem：

A message containing letters from `A-Z` can be **encoded** into numbers using the following mapping:

```markdown
'A' -> "1"
'B' -> "2"
...
'Z' -> "26"
```

To decode an encoded message, all the digits must be grouped then mapped back into letters using the reverse of the mapping above (there may be multiple ways). For example, "11106" can be mapped into:

"AAJF" with the grouping (1 1 10 6)
"KJF" with the grouping (11 10 6)
Note that the grouping (1 11 06) is invalid because "06" cannot be mapped into 'F' since "6" is different from "06".

Given a string s containing only digits, return the number of ways to decode it.

The answer is guaranteed to fit in a 32-bit integer.

Example：

```markdown
Input: s = "12"
Output: 2
```

Explanation: "12" could be decoded as "AB" (1 2) or "L" (12).

```js
/**
 * @param {string} s
 * @return {number}
 */
var numDecodings = function(s) {
    if (s == "0") {
        return 0;
    }
    let _map = new Map();
    let dfs = function(index) {
        if (_map.has(index)) {
            return _map.get(index);
        }
        if (index >= s.length) {
            return 1;
        }
        let v = Number(s.substring(index,index + 2));
        if ((v >= 11 && v <= 19) || (v>=21 && v<=26)) {
            let t = dfs(index+1) + dfs(index+2);
            if (!_map.has(index)) {
                _map.set(index, t);
            }
            return t;
        } else if (s[index] == "0") {
            _map.set(index, 0);
            return 0;
        } else if ([10,20].includes(v)) {
            let t = dfs(index+2);
            if (!_map.has(index)) {
                _map.set(index, t);
            }
            return t;
        } else {
            let t = dfs(index+1);
            if (!_map.has(index)) {
                _map.set(index, t);
            }
            return t;
        }
    }
    return dfs(0);
};
```

