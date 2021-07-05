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



