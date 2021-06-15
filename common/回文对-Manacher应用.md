### **回文对-Manacher应用**

原题描述：给定一组 **互不相同** 的单词， 找出所有**不同** 的索引对`(i, j)`，使得列表中的两个单词， `words[i] + words[j]` ，可拼接成回文串。

题意非常简单，就是在给定的字符串数组里两两组合，算出所有能组成回文对的元素。

比如：

```markdown
输入：["abcd","dcba","lls","s","sssll"]
输出：[[0,1],[1,0],[3,2],[2,4]] 
解释：可拼接成的回文串为 ["dcbaabcd","abcddcba","slls","llssssll"]
```

判断是否回文复杂度为O(m)，m为字符串长度，那么对于暴力解法，两两组合再去判断字符串是否是回文串的整体复杂度为O(n^2*m)；对于一个标记为困难的题，数据量应该是偏大的，因此这并不能作为一个最终解法，和之前我们可以通过使用暴力法去判断超时触发时的数据量；

使用以下暴力方法：

```js
/**
 * @param {string[]} words
 * @return {number[][]}
 */
var palindromePairs = function(words) {
    let just = function(s1, s2) {
        let s = s1+s2;
        for (let i=0; i<s.length/2; i++) {
            if (s[i] != s[s.length-1-i]) {
                return false;
            }
        }
        return true;
    }
    let res = [];
    for (let i=0; i<words.length; i++) {
        for (let j=0; j<words.length; j++) {
            if (i != j && just(words[i], words[j])) {
                res.push([i,j]);
            }
        }
    }
    return res;
};
```

【震惊】竟然通过了，不过消耗时长达到3600ms。

对以上方法进行优化，换个角度。假设数组中有字符串 "ABCD······"， 我们想要找出是否有个字符串满足，我们只需要：

- 判断数组里是否有"A"的后缀，且"BCD······"是否为回文串；
- 判断数组里是否有"BA"的后缀，且"CD······"是否为回文串；
- 判断数组里是否有"CBA"的后缀，且"D······"是否为回文串；
- 依次类推。(这只是枚举前缀，判断后缀是否回文, 当然也需要枚举后缀)；

对于是否包含“XXX”后缀，可以牺牲空间利用Map来存储所给数组的元素的反转字符串, 所以查询的复杂度降到了O(1)； 

代码如下

```js
/**
 * @param {string[]} words
 * @return {number[][]}
 */
var just = function(s) {
    for (let i=0; i<s.length/2; i++) {
        if (s[i] != s[s.length-1-i]) {
            return false;
        }
    }
    return true;
}

var palindromePairs = function(words) {
    
    let res = [];
    let _map = new Map();
    // 反转元素Map
    for (let i=0; i<words.length; i++) {
        _map.set(words[i].split("").reverse().join(""), i);
    }
    for (let i = 0; i < words.length; i++) {
        let m = words[i].length;
        if (!m) {
            continue;
        }
        for (let j = 0; j <= m; j++) {
            
            // 前缀枚举
            let v = _map.get(words[i].substring(0, j));
            if (v != undefined && v != i) {
                if (just(words[i].substring(j, m))) {
                    res.push([i, v]);
                }
            }
            
            // 后缀枚举
            v = _map.get(words[i].substring(j, m));
            if (v != undefined && v != i) {
                if (j && just(words[i].substring(0, j ))) {
                    res.push([v, i]);
                }
            }
        }
    }
    return res;
};
```

优化后的复杂度降到到了O(n * m^2)，同样的样例数据所耗时也降到了300ms。

【再次优化】

在使用了枚举前后缀优化后，我们可以看到，使用枚举前后缀，来计算是否构成回文，内层复杂度为O(m^2)。

可以使用Manacher思想线性求解出所有需要枚举值前/后缀是否为回文串。

manacher代码：

```js
function manacher(s) {
    let n = s.length;
    let tmp = '#'+ s.split('').join("*") +'!';
    let m = n * 2;
    let dp = [];
    let ret = [];
    let p = 0, max = -1;
    for (let i = 1; i < m; i++) {
        dp[i] = max >= i ? Math.min(dp[2 * p - i], max - i) : 0;
        while (tmp[i - dp[i] - 1] == tmp[i + dp[i] + 1] && i + dp[i] + 1 < m) {
            dp[i] += 1;
        }
        if (i + dp[i] > max) {
            p = i, max = i + dp[i];
        }
        if (i - dp[i] == 1) {
            // 前缀标记
            if (ret[Math.floor((i + dp[i]) / 2)] == undefined) {
                ret[Math.floor((i + dp[i]) / 2)] = []
            }
            ret[Math.floor((i + dp[i]) / 2)][0] = true;
        }
        if (i + dp[i] == m - 1) {
            // 后缀标记
            if (ret[Math.floor((i - dp[i]) / 2)] == undefined) {
                ret[Math.floor((i - dp[i]) / 2)] = []
            }
            ret[Math.floor((i - dp[i]) / 2)][1] = true;
        }
    }
    return ret;
}
```

ret为做过标记的数组，我们将内层循环替换成ret结果遍历;

```js
var palindromePairs = function(words) {
    let res = [];
    let _map = new Map();
    // 反转元素Map
    for (let i=0; i<words.length; i++) {
        _map.set(words[i].split("").reverse().join(""), i);
    }
    for (let i = 0; i < words.length; i++) {
        let m = words[i].length;
        if (!m) {
            continue;
        }
        let ret = manacher(words[i]);
        let v = _map.get(words[i]);
        // 全包的情况
        if (v != undefined && v != i) {
            res.push([i, v]);
        }
        for (let j = 0; j <= m; j++) {
            if (!ret[j]) {
                continue;
            }
            if (ret[j][0]) {
                // 满足前缀回文
                v = _map.get(words[i].substring(j+1, m));
                if (v != undefined && v != i) {
                    res.push([v, i]);
                }
            }
            if (ret[j][1]) {
                // 满足后缀回文
                v = _map.get(words[i].substring(0, j));
                if (v != undefined && v != i) {
                    res.push([i, v]);
                }
            }
        }
    }
    return res;
};
```

内层循环替换完整代码如上。不过，这个优化在m期望值比较大的时候才能提现差距。