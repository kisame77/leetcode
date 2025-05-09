# 2508. 添加边使所有节点度数都为偶数
给你一个有 `n` 个节点的 **无向** 图，节点编号为 `1` 到 `n` 。再给你整数 `n` 和一个二维整数数组 `edges` ，其中 <code>edges[i] = [a<sub>i</sub>, b<sub>i</sub>]</code> 表示节点 <code>a<sub>i</sub></code> 和 <code>b<sub>i</sub></code> 之间有一条边。图不一定连通。

你可以给图中添加 **至多** 两条额外的边（也可以一条边都不添加），使得图中没有重边也没有自环。

如果添加额外的边后，可以使得图中所有点的度数都是偶数，返回 `true` ，否则返回 `false` 。

点的度数是连接一个点的边的数目。

#### 示例 1:
![](https://assets.leetcode.com/uploads/2022/10/26/agraphdrawio.png)
<pre>
<strong>输入:</strong> n = 5, edges = [[1,2],[2,3],[3,4],[4,2],[1,4],[2,5]]
<strong>输出:</strong> true
<strong>解释:</strong> 上图展示了添加一条边的合法方案。
最终图中每个节点都连接偶数条边。
</pre>

#### 示例 2:
![](https://assets.leetcode.com/uploads/2022/10/26/aagraphdrawio.png)
<pre>
<strong>输入:</strong> n = 4, edges = [[1,2],[3,4]]
<strong>输出:</strong> true
<strong>解释:</strong> 上图展示了添加两条边的合法方案。
</pre>

#### 示例 3:
![](https://assets.leetcode.com/uploads/2022/10/26/aaagraphdrawio.png)
<pre>
<strong>输入:</strong> n = 4, edges = [[1,2],[1,3],[1,4]]
<strong>输出:</strong> false
<strong>解释:</strong> 无法添加至多 2 条边得到一个符合要求的图。
</pre>

#### 提示:
* <code>3 <= n <= 10<sup>5</sup></code>
* <code>2 <= edges.length <= 10<sup>5</sup></code>
* `edges[i].length == 2`
* <code>1 <= a<sub>i</sub>, b<sub>i</sub> <= n</code>
* <code>a<sub>i</sub> != b<sub>i</sub></code>
* 图中不会有重边

## 题解 (Rust)

### 1. 题解
```Rust
use std::collections::HashSet;

impl Solution {
    pub fn is_possible(n: i32, edges: Vec<Vec<i32>>) -> bool {
        let mut edges_set = HashSet::new();
        let mut is_odd_degree = vec![false; n as usize + 1];
        let mut odd_degree = vec![];

        for edge in &edges {
            let (a, b) = (edge[0] as usize, edge[1] as usize);

            edges_set.insert((a.min(b), a.max(b)));
            is_odd_degree[a] = !is_odd_degree[a];
            is_odd_degree[b] = !is_odd_degree[b];
        }

        for i in 1..=n as usize {
            if is_odd_degree[i] {
                odd_degree.push(i);
            }
        }

        if odd_degree.len() == 0 {
            return true;
        } else if odd_degree.len() == 2 {
            let (a, b) = (odd_degree[0], odd_degree[1]);

            if !edges_set.contains(&(a, b)) {
                return true;
            }

            for c in 1..=n as usize {
                if !edges_set.contains(&(a.min(c), a.max(c)))
                    && !edges_set.contains(&(b.min(c), b.max(c)))
                {
                    return true;
                }
            }
        } else if odd_degree.len() == 4 {
            let (a, b, c, d) = (odd_degree[0], odd_degree[1], odd_degree[2], odd_degree[3]);

            if !edges_set.contains(&(a, b)) && !edges_set.contains(&(c, d)) {
                return true;
            }
            if !edges_set.contains(&(a, c)) && !edges_set.contains(&(b, d)) {
                return true;
            }
            if !edges_set.contains(&(a, d)) && !edges_set.contains(&(b, c)) {
                return true;
            }
        }

        false
    }
}
```
