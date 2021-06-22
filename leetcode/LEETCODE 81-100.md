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

