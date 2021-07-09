### **LEETCODE 341-360**

#### **[349. Intersection of Two Arrays](https://leetcode-cn.com/problems/intersection-of-two-arrays/)**

Problem：

Given two integer arrays nums1 and nums2, return an array of their intersection. Each element in the result must be unique and you may return the result in any order.

Example：

```markdown
Input: nums1 = [4,9,5], nums2 = [9,4,9,8,4]
Output: [9,4]
```

Explanation: [4,9] is also accepted.

```js
/**
 * @param {number[]} nums1
 * @param {number[]} nums2
 * @return {number[]}
 */
var intersection = function(nums1, nums2) {
    let _set = new Set();
    for (let l of nums1) {
        _set.add(l);
    }
    let res = [];
    nums2.forEach((v) => {
        if (_set.has(v)) {
            res.push(v);
            _set.delete(v);
        }
    })
    return res;
};
```
