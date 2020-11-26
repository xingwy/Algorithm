### **LCP 05.LeetCoin-线段树使用**

原题描述：主办方决定给一个刷题团队发`LeetCoin`作为奖励。同时，为了监控给大家发了多少`LeetCoin`，主办方有时候也会进行查询。

该刷题团队的管理模式可以用一棵树表示：

- 团队只有一个负责人，编号为1。除了该负责人外，每个人有且仅有一个领导（负责人没有领导）；
- 不存在循环管理的情况，如A管理B，B管理C，C管理A。

主办方进行的操作有以下三种：

1. 给团队的一个成员（也可以是负责人）发一定数量的`LeetCoin`；
2. 给团队的一个成员（也可以是负责人），以及他/她管理的所有人（即他/她的下属、他/她下属的下属，……），发一定数量的`LeetCoin`；
3. 查询某一个成员（也可以是负责人），以及他/她管理的所有人被发到的`LeetCoin`之和。

输入：

- `N`表示团队成员的个数（编号为1～N，负责人为1）；
- `leadership`是大小为`(N - 1) * 2`的二维数组，其中每个元素`[a, b]`代表`b`是`a`的下属；
- operations是一个长度为Q的二维数组，代表以时间排序的操作，格式如下：
  - operations[i] [0] = 1: 代表第一种操作，operations[i] [1]代表成员的编号，operations[i] [2]代表LeetCoin的数量；
  - operations[i] [0] = 2: 代表第二种操作，operations[i] [1]代表成员的编号，operations[i] [2]代表LeetCoin的数量；
  - operations[i] [0] = 3: 代表第三种操作，operations[i] [1]代表成员的编号；

现在要求是，在有限的时间内，会有N次操作，需要合理的逻辑代码作为性能支持。

#### 分析

如果以多段树去存储领导与下属的关系，那么对于操作①，复杂度为O(1); 由此可见，还是可以满足大数据量的问题，

不过对于操作②和③，如果更新自身需要同步更新自己节点。那么坏的情况，当自己是根节点，那么将更新整棵树，每一步复杂度提升到O(N)级别。作为尝试，可以先实现这一版来验证数据量达到多少时，将会抛出超时提醒。综合①②③，复杂度将会为O(N*M); M为操作次数。

列出节点结构

```js
// 节点的基础结构
class TreeNode {
    index;
    coin = 0;
    next = [];
    constructor (index) {
        this.index = index;
    }
}
```

构建基础树

```js
// Input n: number, leadership: [][], operations: [][]
let tree = [];
for (let i=1; i<=n; i++) {
    tree[i] = new TreeNode();
}

for (let i=0; i<leadership.length; i++) {
    tree[leadership[i][0]].next.push(leadership[i][1]);
}
```

操作定义

```js
// 操作1
let operate1 = function(index, v) {
    let info = tree[index];
    info.coin += v;
}
// 操作2
let operate2 = function(index, v) {
    let info = tree[index];
    for (let i=0; i<info.next.length; i++) {
        operate2(info.next[i], v);
    }
    info.coin = ((info.coin + v)%MOD);
}
// 获取节点
let operate3 = function(index) {
    let info = tree[index];
    let sum = 0;
    for (let i=0; i<info.next.length; i++) {
        sum = (sum + operate3(info.next[i]))%MOD;
    }
    return ((sum + info.coin)%MOD);
}
```

以上为使用传统树的暴力解法，当数据量跑到N = 10^5， M=10^5, 级别，就会出现超时情况。

优化下方法，先了解线段树的定义及功能优势；

假设有编号从1到n的n个点，每个点都存了一些信息，用[L,R]表示下标从L到R的这些点。

线段树的用处就是，对编号连续的一些点进行修改或者统计操作，修改和统计的复杂度都是O(log2(n))。而对于操作2,3。都是对一段内容进行统计和修改。但是，如果使用线段树统计，那么原数据必须符合区间加法。构建如图：

![line_tree1](https://github.com/xingwy/Hugging-Algorithm/blob/master/images/line_tree1.jpg)

​	上图为区间[1, 13]的线段树，对于每一个叶子节点值，在每一层中只出现一次。所以对于每个值的更新，复杂度为线段树层数。

对于求某一个区间的总权值，如图：

![line_tree1](https://github.com/xingwy/Hugging-Algorithm/blob/master/images/line_tree2.jpg)

如果要计算区间[2,12]的总权值，需要统计[2], [3, 4], [5, 7], [8, 10], [11, 12];其实最坏情况下区间个数约为2*log(n)（每一层至多两个区间），所以对于摸一个区间的统计，其复杂度也为O(logn);

（图片资源参考自：[线段树原理](https://blog.csdn.net/zearot/article/details/48299459)）

那么对于此题的需求， 怎么去做一个符合【区间加法】的条件。这里有个很巧妙的转化技巧。如下图：

![line_tree1](https://github.com/xingwy/Hugging-Algorithm/blob/master/images/tree_list_1.jpg)

我们对值1，采用区间表示法，如【入口1，出口1】表示节点①，【入口2，出口2】表示节点②。逢入口时，count标记为1，所以图实例所示，节点已即为【1,6】，节点②表示为【2,4】

映射代码如下：

```js
// Input n: number, leadership: [][], operations: [][]
let _left = [];
let _right = [];
let count = -1;

// 子树集合
let _list = [];
for (let i=0; i<=n; i++) {
    _list[i] = [];
}
// 构建上下级关系列表
for (let i=0; i<leadership.length; ++i) {
    if(!_list[leadership[i][0]]) {}
    _list[leadership[i][0]].push(leadership[i][1]);
}

// dfs只会增加， 不会重置， 那么vout - vin就是直接或间接的下属的数目了
let dfs = function(index) {
    _left[index] = ++count;
    for (let i=0; i<_list[index].length; i++) {
        dfs(_list[index][i]);
    }
    _right[index] = count;
}
dfs(1);
```

通过映射， 可以将index值表示为【left[index], right[index]】。有了符合区间加法的条件，也就很好的对left, right进行线段树构建了。

#### 优化-延时更新

对于操作队列，当我们去更新某个区间时，也会涉及到子区间的更新。其实并不需要实时更新，可以先加数据累积起来（使用val存起来），等到查询子区间的时候再做更新。



总结以上功能，具体实现代码如下：

```js
class LineTree {
    constructor(s, e) {
        // 左子树
        this._left = null;
        // 右子树
        this._right = null;
        // 囊括范围的起点
        this._start = s;
        // 囊括范围的终点
        this._end = e;
        // 囊括范围的中点
        this._mid = Math.floor((s+e)/2);
        this.sum = 0;   //自己的子节点以及间接后代节点总共有多少币
        this.val = 0;   //自己本身总共有多少币
        this.tag = false;   ///是否为叶子节点，true为叶子节点
    }
    // 构建左树
    Left() {
        if (this._start == this._end) return null;
        // 如果有左子树， 就什么都不干， 否则构造一颗新的左子树
        if (!this._left) {
            this._left = new LineTree(this._start, this._mid);
        }
        return this._left;
    }
    // 构建右树
    Right() {
        if (this._start == this._end) return null;
        // 如果有右子树， 就什么都不干， 否则构造一棵新的右子树 
        if (!this._right) {
            this._right = new LineTree(this._mid+1, this._end);
        }
        return this._right;
    }
    
    // 更新人员余额
    update(start, end, newVal) {
        // 自身包括下级的总和更新
        this.sum += (newVal*(end-start+1)); 
        this.sum %= BASE;

        // 如果要更新某个节点的全部后代节点包括自己， 那么前面已经记录了后代节点要更新的， 这里记录一下自己有多少币
        if (this._start == start && this._end == end){
            this.val += newVal;
            return;
        }

        if(this.tag) {
            // 叶子节点 没有子节点
            this.Left().update(start, this._mid, 0);
            this.Right().update(start, this._mid, 0);
        }


        if (end <= this._mid) {
            this.Left().update(start, end, newVal);
        } else if(start >= this._mid+1) {
            this.Right().update(start, end, newVal);
        } else {
            this.Left().update(start, this._mid, newVal);
            this.Right().update(this._mid+1, end, newVal);
        }
    }
    query(start, end, add) {
        let res = 0;
        // 完全节点的情形 直接返回
        if (this._start == start && this._end == end){
            // 当前总数 + 传下来
            res = this.sum + add*(end-start+1);
            res %= BASE;
            return res;
        } 
        add += this.val;
        if (this.tag) {
            res = add*(end-start+1);
            res %= BASE;
            return res; 
        }

        if (end <= this._mid){
            return this.Left().query(start, end,add);
        } else if (start >= this._mid+1) {
            return this.Right().query(start, end, add);
        }
        return (this.Left().query(start, this._mid, add) + this.Right().query(this._mid+1, end, add)) % BASE;
    }
}
```





