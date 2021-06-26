### **LEETCODE 121-140**

#### **[121.Best Time to Buy and Sell Stock](https://leetcode-cn.com/problems/best-time-to-buy-and-sell-stock/)**

Problem：

You are given an array prices where prices[i] is the price of a given stock on the ith day.

You want to maximize your profit by choosing a single day to buy one stock and choosing a different day in the future to sell that stock.

Return the maximum profit you can achieve from this transaction. If you cannot achieve any profit, return 0.

Example：

![tmp-tree_104](https://github.com/xingwy/Hugging-Algorithm/blob/master/images/tmp-tree_104.jpg)

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

