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

#### **[133. Clone Graph](https://leetcode-cn.com/problems/clone-graph/)**

Problem：

Given a reference of a node in a connected undirected graph.

Return a deep copy (clone) of the graph.

Each node in the graph contains a value (int) and a list (List[Node]) of its neighbors.

```c++
class Node {
    public int val;
    public List<Node> neighbors;
}
```

**Test case format:**

For simplicity, each node's value is the same as the node's index (1-indexed). For example, the first node with val == 1, the second node with val == 2, and so on. The graph is represented in the test case using an adjacency list.

An adjacency list is a collection of unordered lists used to represent a finite graph. Each list describes the set of neighbors of a node in the graph.

The given node will always be the first node with val = 1. You must return the copy of the given node as a reference to the cloned graph.

Example：

![133_clone_graph_question](https://github.com/xingwy/Hugging-Algorithm/blob/master/images/133_clone_graph_question.png)

```markdown
Input: adjList = [[2,4],[1,3],[2,4],[1,3]]
Output: [[2,4],[1,3],[2,4],[1,3]]
```

Explanation: 

There are 4 nodes in the graph.

- 1st node (val = 1)'s neighbors are 2nd node (val = 2) and 4th node (val = 4).
- 2nd node (val = 2)'s neighbors are 1st node (val = 1) and 3rd node (val = 3).
- 3rd node (val = 3)'s neighbors are 2nd node (val = 2) and 4th node (val = 4).
- 4th node (val = 4)'s neighbors are 1st node (val = 1) and 3rd node (val = 3).

```js
/**
 * // Definition for a Node.
 * function Node(val, neighbors) {
 *    this.val = val === undefined ? 0 : val;
 *    this.neighbors = neighbors === undefined ? [] : neighbors;
 * };
 */

/**
 * @param {Node} node
 * @return {Node}
 */
var cloneGraph = function(node) {
    let head = new Node();
    let _map = new Map();
    
    let dfs = function(n) {
        if (!n) {
            return;
        }
        if (_map.has(n.val)) {
            return _map.get(n.val);
        }
        let tmp = new Node(n.val);
        _map.set(n.val, tmp);
        for (let neighbor of n.neighbors) {
            tmp.neighbors.push(dfs(neighbor));
        }
        return tmp;
    }
    return dfs(node);
};
```

#### **[135. Candy](https://leetcode-cn.com/problems/candy/)**

Problem：

There are n children standing in a line. Each child is assigned a rating value given in the integer array ratings.

You are giving candies to these children subjected to the following requirements:

- Each child must have at least one candy.
- Children with a higher rating get more candies than their neighbors.

Return the minimum number of candies you need to have to distribute the candies to the children.

Example：

```markdown
Input: ratings = [1,0,2]
Output: 5
```

Explanation: You can allocate to the first, second and third child with 2, 1, 2 candies respectively.

```js
/**
 * @param {number[]} ratings
 * @return {number}
 */
 var candy = function(ratings) {
    let dp  = [];
    let index = 1;

    for (let i=0; i<ratings.length; i++) {
        if (i == 0) {
            dp[i] = 1;
        } else {
            if (ratings[i] <= ratings[i-1]) {
                dp[i] = 1;
            } else {
                dp[i] = dp[i-1]+1;
            }
        }
    }
    let result = 0;
    for (let i=ratings.length-1; i>=0; i--) {
        if (i == ratings.length-1) {
            dp[i] = Math.max(1, dp[i]);
        } else {
            if (ratings[i] <= ratings[i+1]) {
                dp[i] = Math.max(1, dp[i]);
            } else {
                dp[i] = Math.max(dp[i], dp[i+1]+1);
            }
        }
        result += dp[i];
    }
    return result; 
};
```

#### **[136. Single Number](https://leetcode-cn.com/problems/single-number/)**

Problem：

Given a non-empty array of integers nums, every element appears twice except for one. Find that single one.

You must implement a solution with a linear runtime complexity and use only constant extra space.

Example：

```markdown
Input: nums = [2,2,1]
Output: 1
```

```js
/**
 * @param {number[]} nums
 * @return {number}
 */
var singleNumber = function(nums) {
    let t = 0;
    for (let i=0; i<nums.length; i++) {
        t = t ^ nums[i];
    }
    return t;
};
```



