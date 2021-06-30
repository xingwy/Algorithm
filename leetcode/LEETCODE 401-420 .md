### **LEETCODE 401-420**

#### **[403. Frog Jump](https://leetcode-cn.com/problems/frog-jump/)**

Problem：
A frog is crossing a river. The river is divided into some number of units, and at each unit, there may or may not exist a stone. The frog can jump on a stone, but it must not jump into the water.

Given a list of stones' positions (in units) in sorted ascending order, determine if the frog can cross the river by landing on the last stone. Initially, the frog is on the first stone and assumes the first jump must be 1 unit.

If the frog's last jump was k units, its next jump must be either k - 1, k, or k + 1 units. The frog can only jump in the forward direction.

Example：

```markdown
Input: stones = [0,1,3,5,6,8,12,17]
Output: true
```

Explanation: The frog can jump to the last stone by jumping 1 unit to the 2nd stone, then 2 units to the 3rd stone, then 2 units to the 4th stone, then 3 units to the 6th stone, 4 units to the 7th stone, and 5 units to the 8th stone.

```js
/**
 * @param {number[]} stones
 * @return {boolean}
 */
 var canCross = function(stones) {
    let len = stones.length;
    let hash = new Map();
    stones.forEach((v) => {
        hash.set(v, new Set());
    })

    for (let i=0; i<len-1; i++) {
        if (i == 0) {
            if (!hash.has(stones[i]+1)) {
                // 无法到达
                return false;
            } else {
                hash.get(stones[i]+1).add(1);
            }
        } else {
            let step = hash.get(stones[i]);
            if (!step) {
                continue;
            }

            for (let v of step) {
                if (v > 1) {
                    if (hash.has(stones[i] + v - 1)) {
                        hash.get(stones[i] + v - 1).add(v - 1);
                    }
                }
                if (hash.has(stones[i] + v)) {
                    hash.get(stones[i] + v).add(v);
                }
                if (hash.has(stones[i] + v + 1)) {
                    hash.get(stones[i] + v + 1).add(v + 1);
                }

            }
        }
        hash.delete(stones[i])
    }
    return hash.get(stones[len-1]).size > 0;
};

```
