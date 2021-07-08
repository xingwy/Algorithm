### **LEETCODE 221-240**

#### **[223. Rectangle Area](https://leetcode-cn.com/problems/rectangle-area/)**

Problem：

Given the coordinates of two rectilinear rectangles in a 2D plane, return the total area covered by the two rectangles.

The first rectangle is defined by its bottom-left corner (ax1, ay1) and its top-right corner (ax2, ay2).

The second rectangle is defined by its bottom-left corner (bx1, by1) and its top-right corner (bx2, by2).

Example：

![rectangle-plane](https://github.com/xingwy/Hugging-Algorithm/blob/master/images/rectangle-plane.png)

```markdown
Input: ax1 = -3, ay1 = 0, ax2 = 3, ay2 = 4, bx1 = 0, by1 = -1, bx2 = 9, by2 = 2
Output: 45
```

```js
/**
 * @param {number} A
 * @param {number} B
 * @param {number} C
 * @param {number} D
 * @param {number} E
 * @param {number} F
 * @param {number} G
 * @param {number} H
 * @return {number}
 */
var computeArea = function(A, B, C, D, E, F, G, H) {
    let area = (C-A)*(D-B) + (H-F)*(G-E);
    let minx = Math.max(A, E);
    let miny = Math.max(B, F);
    let maxx = Math.min(C, G);
    let maxy = Math.min(D, H);
    if (minx > maxx || miny > maxy) {
        return area;
    } else {
        return area - (maxy - miny)*(maxx - minx);
    }
};
```

#### **[233. Number of Digit One](https://leetcode-cn.com/problems/number-of-digit-one/)**

Problem：

Given an integer n, count the total number of digit 1 appearing in all non-negative integers less than or equal to n.

Example：

```markdown
Input: n = 13
Output: 6
```

```js
/**
 * @param {number} n
 * @return {number}
 */
var countDigitOne = function(n) {
    //前置位
    let prev = n;
    let tail = 0;
    //当前位
    let cur = 0;
    let count = 0;
    let num = 1;
    while (prev != 0) {
        cur = prev % 10;
        prev = Math.floor(prev/10);
        if (cur == 0) {
            // 借位
            count += prev * num;
        } else if (cur == 1) {
            count += (prev * num + (tail + 1));
        } else {
            count += (prev * num + num);
        }

        //尾巴
        tail = cur * num + tail;                  
        num *= 10;
    }
    return count;
};
```

