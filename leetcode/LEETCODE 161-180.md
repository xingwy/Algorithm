### **LEETCODE 161-180**

#### **[164. Maximum Gap](https://leetcode-cn.com/problems/maximum-gap/)**

Problem：

Given an integer array nums, return the maximum difference between two successive elements in its sorted form. If the array contains less than two elements, return 0.

You must write an algorithm that runs in linear time and uses linear extra space.

Example：

```markdown
Input: nums = [3,6,9,1]
Output: 3
```

Explanation: The sorted form of the array is [1,3,6,9], either (3,6) or (6,9) has the maximum difference 3.

```js
/**
 * @param {number[]} nums
 * @return {number}
 */
 var maximumGap = function(nums) {
    if (nums.length < 2) {
        return 0;
    }
    let buts1 = [];
    let buts2 = [];

    for (let i=0; i<=9; i++) {
        buts1[i] = [];
        buts2[i] = []
    }
    for (let base = 1; base <= Math.pow(2, 31); base *= 10) {
        if (base == 1) {
            // 初始化桶1
            for (let i=0; i<nums.length; i++) {
                let id = Math.floor((nums[i]%(base*10))/base);
                buts2[id].push(nums[i]);
            }
        } else {
            // 桶1移植到桶2
            for (let i=0; i<=9; i++) {
                for (let j=0; j<buts1[i].length; j++) {
                    let id = Math.floor((buts1[i][j]%(base*10))/base);
                    buts2[id].push(buts1[i][j]);
                }
            }
        }

        // 初始化桶2  赋值桶1数据
        buts1 = buts2;
        buts2 = [];
        for (let i=0; i<=9; i++) {
            buts2[i] = []
        }
    }
    let max = -Infinity;
    for (let i=0; i<buts1[0].length-1; i++) {
        max = Math.max(max, buts1[0][i+1] - buts1[0][i]);
    }
    return max;
};
```



#### **[167. Two Sum II - Input array is sorted](https://leetcode-cn.com/problems/two-sum-ii-input-array-is-sorted/)**

Problem：

Given an array of integers numbers that is already sorted in non-decreasing order, find two numbers such that they add up to a specific target number.

Return the indices of the two numbers (1-indexed) as an integer array answer of size 2, where 1 <= answer[0] < answer[1] <= numbers.length.

The tests are generated such that there is exactly one solution. You may not use the same element twice.

Example：

```markdown
Input: numbers = [2,7,11,15], target = 9
Output: [1,2]
```

Explanation: The sum of 2 and 7 is 9. Therefore index1 = 1, index2 = 2.

```js
/**
 * @param {number[]} numbers
 * @param {number} target
 * @return {number[]}
 */
var twoSum = function(numbers, target) {
    let l = 0;
    let r = numbers.length-1;
    while(l<r) {
        if (numbers[l] + numbers[r] == target) {
            return [l+1,r+1];
        } else {
            numbers[l]+numbers[r] > target? r--: l++;
        }
    }
    return [-1,-1]
};
```

#### **[171. Excel Sheet Column Number](https://leetcode-cn.com/problems/excel-sheet-column-number/)**

Problem：

Given a string columnTitle that represents the column title as appear in an Excel sheet, return its corresponding column number.

```markdown
A -> 1
B -> 2
C -> 3
...
Z -> 26
AA -> 27
AB -> 28 
...
```

Example：

```markdown
Input: columnTitle = "FXSHRXW"
Output: 2147483647
```

```js
/**
 * @param {string} s
 * @return {number}
 */
var titleToNumber = function(s) {
    let sum = 0;
    for (let i=0; i<s.length; i++) {
        sum = (sum*26 + s[i].charCodeAt()-'A'.charCodeAt()+1);  
    }
    return sum;
};
```

#### **[172. Factorial Trailing Zeroes](https://leetcode-cn.com/problems/factorial-trailing-zeroes/)**

Problem：

Given an integer n, return the number of trailing zeroes in n!.

Follow up: Could you write a solution that works in logarithmic time complexity?

Example：

```markdown
Input: n = 3
Output: 0
```

Explanation: 3! = 6, no trailing zero.

```js
/**
 * @param {number} n
 * @return {number}
 */
var trailingZeroes = function(n) {
    let num = 0;
    let base = 5;
    while (n >= base) {
        num += Math.floor(n/base);
        base *= 5;
    }
    return num;
};
```



#### **[174. Dungeon Game](https://leetcode-cn.com/problems/dungeon-game/)**

Problem：
The demons had captured the princess and imprisoned her in **the bottom-right corner** of a `dungeon`. The `dungeon` consists of `m x n` rooms laid out in a 2D grid. Our valiant knight was initially positioned in **the top-left room** and must fight his way through `dungeon` to rescue the princess.

The knight has an initial health point represented by a positive integer. If at any point his health point drops to `0` or below, he dies immediately.

Some of the rooms are guarded by demons (represented by negative integers), so the knight loses health upon entering these rooms; other rooms are either empty (represented as 0) or contain magic orbs that increase the knight's health (represented by positive integers).

To reach the princess as quickly as possible, the knight decides to move only **rightward** or **downward** in each step.

Return *the knight's minimum initial health so that he can rescue the princess*.

**Note** that any room can contain threats or power-ups, even the first room the knight enters and the bottom-right room where the princess is imprisoned.

Example：

![dungeon-grid-1](https://github.com/xingwy/Hugging-Algorithm/blob/master/images/dungeon-grid-1.jpg)

```markdown
Input: dungeon = [[-2,-3,3],[-5,-10,1],[10,30,-5]]
Output: 7
```

Explanation: The initial health of the knight must be at least 7 if he follows the optimal path: RIGHT-> RIGHT -> DOWN -> DOWN.

```js
/**
 * @param {number[][]} dungeon
 * @return {number}
 */
 var calculateMinimumHP = function(dungeon) {
    let m = dungeon.length-1;
    if (m < 0) {
        return 1;
    }
    let n = dungeon[0].length-1;
    
    let dp =[];
    dp[m] = []
    dp[m][n] = Math.max(1-dungeon[m][n], 1);

    // 初始化底行
    for (let i=n-1; i>=0; i--) {
        dp[m][i] = Math.max(dp[m][i+1] - dungeon[m][i], 1);
    }
	// 初始化最后列
    for (let i=m-1; i>=0; i--) {
        dp[i] = [];
        dp[i][n] = Math.max(dp[i+1][n] - dungeon[i][n], 1);
    }
	
    for (let i=m-1; i>=0; i--) {
        for (let j=n-1; j>=0; j--) {
            let min = Math.min(dp[i+1][j], dp[i][j+1]);
            dp[i][j] = Math.max(min - dungeon[i][j], 1);
        }   
    }
    return dp[0][0];
};
```
