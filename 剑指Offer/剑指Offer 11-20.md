### **LCP 11-20**

#### [剑指 Offer 11. 旋转数组的最小数字  LCOF](https://leetcode-cn.com/problems/xuan-zhuan-shu-zu-de-zui-xiao-shu-zi-lcof/)

题：把一个数组最开始的若干个元素搬到数组的末尾，我们称之为数组的旋转。输入一个递增排序的数组的一个旋转，输出旋转数组的最小元素。例如，数组 [3,4,5,1,2] 为 [1,2,3,4,5] 的一个旋转，该数组的最小值为1。  

示例：

```markdown
输入：[3,4,5,1,2]
输出：1
```

```js
/**
 * @param {number[]} numbers
 * @return {number}
 */
var minArray = function(numbers) {
    let l = 0;
    let r = numbers.length-1;
    while (l<r) {
        let mid = Math.floor((l+r)/2);
        if (numbers[mid] < numbers[r]) {
            r = mid;
        } else if(numbers[mid] > numbers[r]) {
            l = mid + 1;
        } else {
            r--;
        }
    }
    return numbers[r];
};
```
