### **LEETCODE 161-180**

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
