### **LEETCODE 441-460**

#### **[458. Poor Pigs](https://leetcode-cn.com/problems/poor-pigs/)**

Problem：
There are buckets buckets of liquid, where exactly one of the buckets is poisonous. To figure out which one is poisonous, you feed some number of (poor) pigs the liquid to see whether they will die or not. Unfortunately, you only have minutesToTest minutes to determine which bucket is poisonous.

You can feed the pigs according to these steps:

1. Choose some live pigs to feed.
2. For each pig, choose which buckets to feed it. The pig will consume all the chosen buckets simultaneously and will take no time.
3. Wait for minutesToDie minutes. You may not feed any other pigs during this time.
4. After minutesToDie minutes have passed, any pigs that have been fed the poisonous bucket will die, and all others will survive.
5. Repeat this process until you run out of time.

Given buckets, minutesToDie, and minutesToTest, return the minimum number of pigs needed to figure out which bucket is poisonous within the allotted time.

Example：

```markdown
Input: buckets = 1000, minutesToDie = 15, minutesToTest = 60
Output: 5
```

```js
/**
 * @param {number} buckets
 * @param {number} minutesToDie
 * @param {number} minutesToTest
 * @return {number}
 */
var poorPigs = function(buckets, minutesToDie, minutesToTest) {
    return Math.ceil(Math.log2(buckets)/Math.log2(Math.floor(minutesToTest/minutesToDie) + 1));
};
```
