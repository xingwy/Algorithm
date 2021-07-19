### **LEETCODE 481-500**

#### **[483. Smallest Good Base](https://leetcode-cn.com/problems/smallest-good-base/)**

Problem：
Given an integer n represented as a string, return the smallest good base of n.

We call k >= 2 a good base of n, if all digits of n base k are 1's.

Example：

```markdown
Input: n = "13"
Output: "3"
```

Explanation: 13 base 3 is 111.

```js
/**
 * @param {string} n
 * @return {string}
 */
var smallestGoodBase = function(n) {
    let now = BigInt(n);
    let MAX = now + 10n;
    let pow = function(k,n) {
        let t = 1n;
        for (let i=1n; i<n; i++) {
            t = t*k;
            if ((t - 1n) / (k - 1n) > now) {
                return (k - 1n) * MAX + 1n;
            }
        }
        return t;
    }
    let cal = function(k, n) {
        return (pow(k, n + 1n) - 1n) / (k - 1n);
    }
    for (let i=55n; i>=3n; i--) {
        let l = 2n;
        let n_t = Math.min(Math.ceil(Math.pow(Number(String(now)), 1/Number(String(i - 1n)))).toString(), Number(String(n))).toString();
        let r = BigInt(n_t);
        while(r >= l) {
            let o = l + r;
            if(o%2n != 0n){
                o += 1n;
            }
            let mid = o / 2n;
            let c = cal(mid, i);
            if (c == now) {
                return String(mid);
            } else {
                if (c >= n) {
                    r = mid - 1n;
                } else {
                    l = mid + 1n;
                }
            }
        }
    }
    return (now-1n).toString();
};
```

