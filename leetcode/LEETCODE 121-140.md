### **LEETCODE 121-140**

#### **[121.Best Time to Buy and Sell Stock](https://leetcode-cn.com/problems/best-time-to-buy-and-sell-stock/)**

Problem：

You are given an array prices where prices[i] is the price of a given stock on the ith day.

You want to maximize your profit by choosing a single day to buy one stock and choosing a different day in the future to sell that stock.

Return the maximum profit you can achieve from this transaction. If you cannot achieve any profit, return 0.

Example：

```markdown
Input: prices = [7,1,5,3,6,4]
Output: 5
```

Explanation: Buy on day 2 (price = 1) and sell on day 5 (price = 6), profit = 6-1 = 5.
Note that buying on day 2 and selling on day 1 is not allowed because you must buy before you sell.

```js
/**
 * @param {number[]} prices
 * @return {number}
 */
var maxProfit = function(prices) {
    let money = 0;
    let buy = prices[0];

    for (let i=1; i<prices.length; i++) {
        if (prices[i] > buy) {
            money = Math.max(money, prices[i] - buy);
        }
        buy = Math.min(buy, prices[i])
    }
    return money;
};
```

#### **[122.Best Time to Buy and Sell Stock II](https://leetcode-cn.com/problems/best-time-to-buy-and-sell-stock-ii/)**

Problem：

You are given an array prices where prices[i] is the price of a given stock on the ith day.

Find the maximum profit you can achieve. You may complete as many transactions as you like (i.e., buy one and sell one share of the stock multiple times).

Note: You may not engage in multiple transactions simultaneously (i.e., you must sell the stock before you buy again).

Example：

```markdown
Input: prices = [7,1,5,3,6,4]
Output: 7
```

Explanation: 

Buy on day 2 (price = 1) and sell on day 3 (price = 5), profit = 5-1 = 4.
Then buy on day 4 (price = 3) and sell on day 5 (price = 6), profit = 6-3 = 3.

```js
/**
 * @param {number[]} prices
 * @return {number}
 */
var maxProfit = function(prices) {
    let min = 0;
    let max = 0;
    let res = 0;
    for (let i=0; i<prices.length; i++) {
        if (i == 0) {
            min = prices[i];
            max = prices[i];
        } else {
            if (prices[i] < min || prices[i] < max) {
                res += (max - min);
                min = max = prices[i];
            } else if (prices[i] >= max) {
                max = prices[i];
            }
        }
    }
    return res + max - min;
};
```

#### **[123.Best Time to Buy and Sell Stock III](https://leetcode-cn.com/problems/best-time-to-buy-and-sell-stock-iii/)**

Problem：

You are given an array prices where prices[i] is the price of a given stock on the ith day.

Find the maximum profit you can achieve. You may complete at most two transactions.

Note: You may not engage in multiple transactions simultaneously (i.e., you must sell the stock before you buy again).

Example：

```markdown
Input: prices = [3,3,5,0,0,3,1,4]
Output: 6
```

Explanation: 

Buy on day 4 (price = 0) and sell on day 6 (price = 3), profit = 3-0 = 3.
Then buy on day 7 (price = 1) and sell on day 8 (price = 4), profit = 4-1 = 3.

```js\
/**
 * @param {number[]} prices
 * @return {number}
 */
var maxProfit = function(prices) {
    let dp = [[[],[],[]]];
    dp[0][0][0] = 0;
    dp[0][0][1] = 0;
    dp[0][0][2] = 0;

    dp[0][1][0] = -prices[0];
    dp[0][1][1] = -prices[0];
    dp[0][1][2] = -prices[0];


    for (let i=1; i<prices.length; i++) {   
        dp[i] = [[],[],[]];
        // 更新 今天未有股票
        dp[i][0][0] = 0;
        dp[i][0][1] = Math.max(dp[i-1][0][1], dp[i-1][1][0] + prices[i]);
        dp[i][0][2] = Math.max(dp[i-1][0][2], dp[i-1][1][1] + prices[i]);

        // 今天持有
        dp[i][1][0] = Math.max(dp[i-1][1][0], dp[i-1][0][0] - prices[i]);
        dp[i][1][1] = Math.max(dp[i-1][1][1], dp[i-1][0][1] - prices[i]);
        dp[i][1][2] = 0;
    }

    return prices.length == 0? 0 : Math.max(dp[prices.length-1][0][2], dp[prices.length-1][0][1]);
};
```

#### **[124. Binary Tree Maximum Path Sum](https://leetcode-cn.com/problems/binary-tree-maximum-path-sum/)**

Problem：

A path in a binary tree is a sequence of nodes where each pair of adjacent nodes in the sequence has an edge connecting them. A node can only appear in the sequence at most once. Note that the path does not need to pass through the root.

The path sum of a path is the sum of the node's values in the path.

Given the root of a binary tree, return the maximum path sum of any path.

Example：

![exx2](https://github.com/xingwy/Hugging-Algorithm/blob/master/images/exx2.jpg)

```markdown
Input: root = [-10,9,20,null,null,15,7]
Output: 42
```

Explanation: The optimal path is 15 -> 20 -> 7 with a path sum of 15 + 20 + 7 = 42.

```js
/**
 * Definition for a binary tree node.
 * function TreeNode(val, left, right) {
 *     this.val = (val===undefined ? 0 : val)
 *     this.left = (left===undefined ? null : left)
 *     this.right = (right===undefined ? null : right)
 * }
 */
/**
 * @param {TreeNode} root
 * @return {number}
 */
var maxPathSum = function(root) {
    let max = -Infinity;
    let ra = function (node) {
        if (!node) {
            return 0;
        }

        let left = ra(node.left);
        let right = ra(node.right);

        max = Math.max(max, left + right + node.val, left + node.val, node.val, right + node.val);
        return Math.max(left + node.val, right + node.val, 0);
    }
    ra(root);
    return max;
};
```

