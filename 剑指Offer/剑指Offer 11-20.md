### **LCP 11-20**

#### [剑指 Offer 11. 旋转数组的最小数字  LCOF](https://leetcode-cn.com/problems/xuan-zhuan-shu-zu-de-zui-xiao-shu-zi-lcof/)

题：把一个数组最开始的若干个元素搬到数组的末尾，我们称之为数组的旋转。输入一个递增排序的数组的一个旋转，输出旋转数组的最小元素。例如，数组 [3,4,5,1,2] 为 [1,2,3,4,5] 的一个旋转，该数组的最小值为1。  

示例：

```markdown
输入：[3,4,5,1,2]
输出：1
```

```js
/**
 * @param {number[]} numbers
 * @return {number}
 */
var minArray = function(numbers) {
    let l = 0;
    let r = numbers.length-1;
    while (l<r) {
        let mid = Math.floor((l+r)/2);
        if (numbers[mid] < numbers[r]) {
            r = mid;
        } else if(numbers[mid] > numbers[r]) {
            l = mid + 1;
        } else {
            r--;
        }
    }
    return numbers[r];
};
```

#### [剑指 Offer 12. 矩阵中的路径](https://leetcode-cn.com/problems/ju-zhen-zhong-de-lu-jing-lcof/)

题：给定一个 m x n 二维字符网格 board 和一个字符串单词 word 。如果 word 存在于网格中，返回 true ；否则，返回 false 。

单词必须按照字母顺序，通过相邻的单元格内的字母构成，其中“相邻”单元格是那些水平相邻或垂直相邻的单元格。同一个单元格内的字母不允许被重复使用。

例如，在下面的 3×4 的矩阵中包含单词 "ABCCED"（单词中的字母已标出）。

![word2](https://github.com/xingwy/Algorithm/tree/master/images/word2.jpg)

示例：

```markdown
输入：board = [
	["A","B","C","E"],
	["S","F","C","S"],
	["A","D","E","E"]
], word = "ABCCED"
输出：true
```

```js
/**
 * @param {character[][]} board
 * @param {string} word
 * @return {boolean}
 */
var exist = function(board, word) {
    
    for (let i=0; i<board.length; i++) {
        for (let j=0; j<board[i].length; j++) {
            let path = new Set();
            let res = dfs(board, i, j, word, 0, path);
            if (res) {
                return true;
            }
        }
    }
    return false;
};

let dfs = function(board, p_i, p_j, word, curIndex, path) {
    if (curIndex >= word.length) {
        return true;
    }
    if (path.has(p_i+"-"+p_j)) {
        return false;
    }
    if (p_i < 0 || p_i >= board.length || p_j < 0 || p_j >= board[0].length) {
        return false;
    }
    let key = word[curIndex];
    if (board[p_i][p_j] != key) {
        return false;
    }

    path.add(p_i+"-"+p_j);
    let r1 = dfs(board, p_i+1, p_j, word, curIndex+1, path);
    if (r1) {
        path.delete(p_i+"-"+p_j);
        return r1;
    }
    
    let r2 = dfs(board, p_i-1, p_j, word, curIndex+1, path);
    if (r2) {
        path.delete(p_i+"-"+p_j);
        return r2;
    }
    let r3 = dfs(board, p_i, p_j+1, word, curIndex+1, path);

    if (r3) {
        path.delete(p_i+"-"+p_j);
        return r3;
    }
    let r4 = dfs(board, p_i, p_j-1, word, curIndex+1, path);

    if (r4) {
        path.delete(p_i+"-"+p_j);
        return r4;
    }
    path.delete(p_i+"-"+p_j);
    return false;
}
```

