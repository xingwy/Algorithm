### **LEETCODE 781-800**

#### **[797. 所有可能的路径](https://leetcode-cn.com/problems/all-paths-from-source-to-target/)**

问题：

给你一个有 n 个节点的 有向无环图（DAG），请你找出所有从节点 0 到节点 n-1 的路径并输出（不要求按特定顺序）

二维数组的第 i 个数组中的单元都表示有向图中 i 号节点所能到达的下一些节点，空就是没有下一个结点了。

译者注：有向图是有方向的，即规定了 a→b 你就不能从 b→a 。

![all_1](https://github.com/xingwy/Hugging-Algorithm/blob/master/images/all_1.jpg)

Example：

```markdown
输入：graph = [[1,2],[3],[3],[]]
输出：[[0,1,3],[0,2,3]]
解释：有两条路径 0 -> 1 -> 3 和 0 -> 2 -> 3
```

```go
func allPathsSourceTarget(graph [][]int) [][]int {
	var res [][]int
	var n int = len(graph) - 1
	var path = []int{}
	var c func(cur int, index int)
	c = func(cur int, index int) {
		path = append(path, cur)
		if cur == n {
			res = append(res, append([]int(nil), path...))
			return
		}
		var l = len(path)
		for i := 0; i < len(graph[cur]); i++ {
			path = path[:l]
			c(graph[cur][i], index+1)
		}
	}
	c(0, 0)
	return res
}
```

