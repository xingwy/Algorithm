### **LEETCODE 661-680**

#### **[679. 24 Game](https://leetcode-cn.com/problems/24-game/)**

Problem：

You are given an integer array cards of length 4. You have four cards, each containing a number in the range [1, 9]. You should arrange the numbers on these cards in a mathematical expression using the operators ['+', '-', '*', '/'] and the parentheses '(' and ')' to get the value 24.

You are restricted with the following rules:

- The division operator '/' represents real division, not integer division.
  For example, 4 / (1 - 2 / 3) = 4 / (1 / 3) = 12.
- Every operation done is between two numbers. In particular, we cannot use '-' as a unary operator.
  For example, if cards = [1, 1, 1, 1], the expression "-1 - 1 - 1 - 1" is not allowed.
- You cannot concatenate numbers together
  For example, if cards = [1, 2, 1, 2], the expression "12 + 12" is not valid.

Return true if you can get such expression that evaluates to 24, and false otherwise.

Example：

```markdown
Input: cards = [4,1,8,7]
Output: true
```

Explanation: (8-4) * (7-1) = 24

```js
/**
 * @param {number[]} nums
 * @return {boolean}
 */

var judgePoint24 = function(nums) {
    if (nums.length == 1)
        return Math.abs(nums[0] - 24) < 1e-6;
    for (let i = 0; i < nums.length; i++)
        for (let j = i + 1; j < nums.length; j++) {
        let rest = nums.filter((value, index) => index != i && index != j);
        if (judgePoint24([nums[i] + nums[j], ...rest]) || judgePoint24([nums[i] * nums[j], ...rest]) ||
            judgePoint24([nums[i] - nums[j], ...rest]) || judgePoint24([nums[j] - nums[i], ...rest]) ||
            judgePoint24([nums[i] / nums[j], ...rest]) || judgePoint24([nums[j] / nums[i], ...rest]))
            return true;
        }
    return false;
};
```

