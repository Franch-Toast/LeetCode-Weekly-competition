

# No.102 Biweekly competition

## Easy 2639. 查询网格图中每一列的宽度 3 points

给你一个下标从 **0** 开始的 `m x n` 整数矩阵 `grid` 。矩阵中某一列的宽度是这一列数字的最大 **字符串长度** 。

- 比方说，如果 `grid = [[-10], [3], [12]]` ，那么唯一一列的宽度是 `3` ，因为 `-10` 的字符串长度为 `3` 。

请你返回一个大小为 `n` 的整数数组 `ans` ，其中 `ans[i]` 是第 `i` 列的宽度。

一个有 `len` 个数位的整数 `x` ，如果是非负数，那么 **字符串****长度** 为 `len` ，否则为 `len + 1` 。

 

**示例 1：**

```
输入：grid = [[1],[22],[333]]
输出：[3]
解释：第 0 列中，333 字符串长度为 3 。
```

**示例 2：**

```
输入：grid = [[-15,1,3],[15,7,12],[5,6,-2]]
输出：[3,1,2]
解释：
第 0 列中，只有 -15 字符串长度为 3 。
第 1 列中，所有整数的字符串长度都是 1 。
第 2 列中，12 和 -2 的字符串长度都为 2 。
```

 

**提示：**

- `m == grid.length`
- `n == grid[i].length`
- `1 <= m, n <= 100 `
- `-109 <= grid[r][c] <= 109`



### 模拟

我第一次写的方法很复杂也不是很清晰。在不停转换中容易出错，并且手写除法容易出现问题。

```cpp
class Solution {
public:
    vector<int> findColumnWidth(vector<vector<int>>& grid) {
        vector<int> ans;
        
        for(int i=0;i<grid[0].size();i++)//列数
        {
            int num_max=0;
            int num_min=0;
            for(int j=0;j<grid.size();j++)//行数
            {
                num_max=max(num_max,grid[j][i]);
                num_min=min(num_min,grid[j][i]);
            }
            int len=0;
            if(abs(num_max)/10>abs(num_min)) num_min=num_max;
            if(num_min<=0)
            {
                len++;
                num_min=-num_min;
            }
            while(num_min)
            {
                len++;
                num_min/=10;
            }
            ans.push_back(len);
        }
        return ans;
    }
};
```

### 库函数

使用库函数`to_string`的方法会更加清晰也更加简单，不容易出错。但是这里要强制转换，因为`to_string.size()`返回的变量是一个`unsigned long`变量。

```cpp
class Solution {
public:
    vector<int> findColumnWidth(vector<vector<int>>& grid) {
        int m = grid.size(), n = grid[0].size();
        vector<int> ans;
        int mid;
        for (int i = 0; i < n; ++i) {//列
            mid = 0;
            for (int j = 0; j < m; ++j) {//按行扫描
                mid = max(mid, (int)to_string(grid[j][i]).size());
            }
            ans.emplace_back(mid);
        }
        return ans;
    }
};
```



---

## Medium 2640. 一个数组所有前缀的分数 4 points

定义一个数组 `arr` 的 **转换数组** `conver` 为：

- `conver[i] = arr[i] + max(arr[0..i])`，其中 `max(arr[0..i])` 是满足 `0 <= j <= i` 的所有 `arr[j]` 中的最大值。

定义一个数组 `arr` 的 **分数** 为 `arr` 转换数组中所有元素的和。

给你一个下标从 **0** 开始长度为 `n` 的整数数组 `nums` ，请你返回一个长度为 `n` 的数组 `ans` ，其中 `ans[i]`是前缀 `nums[0..i]` 的分数。

 

**示例 1：**

```
输入：nums = [2,3,7,5,10]
输出：[4,10,24,36,56]
解释：
对于前缀 [2] ，转换数组为 [4] ，所以分数为 4 。
对于前缀 [2, 3] ，转换数组为 [4, 6] ，所以分数为 10 。
对于前缀 [2, 3, 7] ，转换数组为 [4, 6, 14] ，所以分数为 24 。
对于前缀 [2, 3, 7, 5] ，转换数组为 [4, 6, 14, 12] ，所以分数为 36 。
对于前缀 [2, 3, 7, 5, 10] ，转换数组为 [4, 6, 14, 12, 20] ，所以分数为 56 。
```

**示例 2：**

```
输入：nums = [1,1,2,4,8,16]
输出：[2,4,8,16,32,64]
解释：
对于前缀 [1] ，转换数组为 [2] ，所以分数为 2 。
对于前缀 [1, 1]，转换数组为 [2, 2] ，所以分数为 4 。
对于前缀 [1, 1, 2]，转换数组为 [2, 2, 4] ，所以分数为 8 。
对于前缀 [1, 1, 2, 4]，转换数组为 [2, 2, 4, 8] ，所以分数为 16 。
对于前缀 [1, 1, 2, 4, 8]，转换数组为 [2, 2, 4, 8, 16] ，所以分数为 32 。
对于前缀 [1, 1, 2, 4, 8, 16]，转换数组为 [2, 2, 4, 8, 16, 32] ，所以分数为 64 。
```

 

**提示：**

- `1 <= nums.length <= 105`
- `1 <= nums[i] <= 109`



### 模拟

只需要一次遍历，一边维护一遍求和即可。

```cpp
class Solution {
public:
    vector<long long> findPrefixScore(vector<int>& nums) {
        vector<long long> ans;

        int cur_max=-INT_MAX;
        long long sum=0;
        for(int num:nums)
        {
            cur_max=max(cur_max,num);
            sum=sum+num+cur_max;
            ans.push_back(sum);
        }
        return ans;
    }
};
```





---

## Medium 2641. 二叉树的堂兄弟节点 II 5 points

给你一棵二叉树的根 `root` ，请你将每个节点的值替换成该节点的所有 **堂兄弟节点值的和** 。

如果两个节点在树中有相同的深度且它们的父节点不同，那么它们互为 **堂兄弟** 。

请你返回修改值之后，树的根 `root` 。

**注意**，一个节点的深度指的是从树根节点到这个节点经过的边数。

 

**示例 1：**

![img](https://assets.leetcode.com/uploads/2023/01/11/example11.png)

```
输入：root = [5,4,9,1,10,null,7]
输出：[0,0,0,7,7,null,11]
解释：上图展示了初始的二叉树和修改每个节点的值之后的二叉树。
- 值为 5 的节点没有堂兄弟，所以值修改为 0 。
- 值为 4 的节点没有堂兄弟，所以值修改为 0 。
- 值为 9 的节点没有堂兄弟，所以值修改为 0 。
- 值为 1 的节点有一个堂兄弟，值为 7 ，所以值修改为 7 。
- 值为 10 的节点有一个堂兄弟，值为 7 ，所以值修改为 7 。
- 值为 7 的节点有两个堂兄弟，值分别为 1 和 10 ，所以值修改为 11 。
```

**示例 2：**

![img](https://assets.leetcode.com/uploads/2023/01/11/diagram33.png)

```
输入：root = [3,1,2]
输出：[0,0,0]
解释：上图展示了初始的二叉树和修改每个节点的值之后的二叉树。
- 值为 3 的节点没有堂兄弟，所以值修改为 0 。
- 值为 1 的节点没有堂兄弟，所以值修改为 0 。
- 值为 2 的节点没有堂兄弟，所以值修改为 0 。
```

 

**提示：**

- 树中节点数目的范围是 `[1, 105]` 。
- `1 <= Node.val <= 104`



### BFS

站在父节点的角度上，求一次下一层结点的和后，再次遍历，将求得的结点的和剪掉父节点的左右子节点即为下一层的兄弟节点的和，十分巧妙。这里用了两个栈，一个栈作为父节点的存放栈，用于两次遍历，另一个作为暂存栈，用于存放下一层结点，等到存放栈的功能完成（两次遍历后），将暂存栈赋给存放栈即可。故而没有传统BFS的弹栈操作。

```cpp
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode() : val(0), left(nullptr), right(nullptr) {}
 *     TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
 *     TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right) {}
 * };
 */
class Solution {
public:
    TreeNode* replaceValueInTree(TreeNode* root) {
        
        vector<TreeNode*> cur={root};
        root->val=0;
        
        while(!cur.empty())
        {
            vector<TreeNode*> next;//下一层的节点保存容器
            int sum=0;
            for(auto node:cur)//第一次遍历，计算下一层结点值的总和
            {
                if(node->left)
                {
                    next.push_back(node->left);
                    sum+=node->left->val;
                }
                if(node->right)
                {
                    next.push_back(node->right);
                    sum+=node->right->val;
                }
            }

            for(auto node:cur)//第二次遍历，计算当前节点的左右子孙节点的和并进行赋值
            {
                int bothson= (node->left ?  node->left->val:0)+(node->right ?  node->right->val:0);
                if(node->left) node->left->val=sum-bothson;
                if(node->right) node->right->val=sum-bothson;
            }
            cur=move(next);
        }
        return root;
    }
};
```





---

## Hard 2642. 设计可以求最短路径的图类 6 points

给你一个有 `n` 个节点的 **有向带权** 图，节点编号为 `0` 到 `n - 1` 。图中的初始边用数组 `edges` 表示，其中 `edges[i] = [fromi, toi, edgeCosti]` 表示从 `fromi` 到 `toi` 有一条代价为 `edgeCosti` 的边。

请你实现一个 `Graph` 类：

- `Graph(int n, int[][] edges)` 初始化图有 `n` 个节点，并输入初始边。
- `addEdge(int[] edge)` 向边集中添加一条边，其中 `edge = [from, to, edgeCost]` 。数据保证添加这条边之前对应的两个节点之间没有有向边。
- `int shortestPath(int node1, int node2)` 返回从节点 `node1` 到 `node2` 的路径 **最小** 代价。如果路径不存在，返回 `-1` 。一条路径的代价是路径中所有边代价之和。

 

**示例 1：**

![img](https://assets.leetcode.com/uploads/2023/01/11/graph3drawio-2.png)

```
输入：
["Graph", "shortestPath", "shortestPath", "addEdge", "shortestPath"]
[[4, [[0, 2, 5], [0, 1, 2], [1, 2, 1], [3, 0, 3]]], [3, 2], [0, 3], [[1, 3, 4]], [0, 3]]
输出：
[null, 6, -1, null, 6]

解释：
Graph g = new Graph(4, [[0, 2, 5], [0, 1, 2], [1, 2, 1], [3, 0, 3]]);
g.shortestPath(3, 2); // 返回 6 。从 3 到 2 的最短路径如第一幅图所示：3 -> 0 -> 1 -> 2 ，总代价为 3 + 2 + 1 = 6 。
g.shortestPath(0, 3); // 返回 -1 。没有从 0 到 3 的路径。
g.addEdge([1, 3, 4]); // 添加一条节点 1 到节点 3 的边，得到第二幅图。
g.shortestPath(0, 3); // 返回 6 。从 0 到 3 的最短路径为 0 -> 1 -> 3 ，总代价为 2 + 4 = 6 。
```

 

**提示：**

- `1 <= n <= 100`
- `0 <= edges.length <= n * (n - 1)`
- `edges[i].length == edge.length == 3`
- `0 <= fromi, toi, from, to, node1, node2 <= n - 1`
- `1 <= edgeCosti, edgeCost <= 106`
- 图中任何时候都不会有重边和自环。
- 调用 `addEdge` 至多 `100` 次。
- 调用 `shortestPath` 至多 `100` 次。





### Dijkstra

```cpp
class Graph {
    vector<vector<int>> g;
public:
    Graph(int n, vector<vector<int>> &edges) {
        // 邻接矩阵（初始化为无穷大，表示 i 到 j 没有边）
        g = vector<vector<int>>(n, vector<int>(n, INT_MAX / 2));
        for (auto &e: edges)
            g[e[0]][e[1]] = e[2]; // 添加一条边（输入保证没有重边）
    }

    void addEdge(vector<int> e) {
        g[e[0]][e[1]] = e[2]; // 添加一条边（输入保证这条边之前不存在）
    }

    // 朴素 Dijkstra 算法
    int shortestPath(int start, int end) {
        int n = g.size();
        vector<int> dis(n, INT_MAX / 2), vis(n);
        dis[start] = 0;
        for (;;) {
            // 找到当前最短路，去更新它的邻居的最短路
            // 根据数学归纳法，dis[x] 一定是最短路长度
            int x = -1;
            for (int i = 0; i < n; ++i)
                if (!vis[i] && (x < 0 || dis[i] < dis[x]))
                    x = i;
            if (x < 0 || dis[x] == INT_MAX / 2) // 所有从 start 能到达的点都被更新了
                return -1;
            if (x == end) // 找到终点，提前退出
                return dis[x];
            vis[x] = true; // 标记，在后续的循环中无需反复更新 x 到其余点的最短路长度
            for (int y = 0; y < n; ++y)
                dis[y] = min(dis[y], dis[x] + g[x][y]); // 更新最短路长度
        }
    }
};
```



### Floyd

```cpp
class Graph {
    vector<vector<int>> d;
public:
    Graph(int n, vector<vector<int>> &edges) {
        // 邻接矩阵（初始化为无穷大，表示 i 到 j 没有边）
        d = vector<vector<int>>(n, vector<int>(n, INT_MAX / 3));
        for (int i = 0; i < n; ++i)
            d[i][i] = 0;
        for (auto &e: edges)
            d[e[0]][e[1]] = e[2]; // 添加一条边（输入保证没有重边和自环）
        for (int k = 0; k < n; ++k)
            for (int i = 0; i < n; ++i)
                for (int j = 0; j < n; ++j)
                    d[i][j] = min(d[i][j], d[i][k] + d[k][j]);
    }

    void addEdge(vector<int> e) {
        int x = e[0], y = e[1], w = e[2], n = d.size();
        if (w >= d[x][y]) // 无需更新
            return;
        for (int i = 0; i < n; ++i)
            for (int j = 0; j < n; ++j)
                d[i][j] = min(d[i][j], d[i][x] + w + d[y][j]);
    }

    int shortestPath(int start, int end) {
        int ans = d[start][end];
        return ans < INT_MAX / 3 ? ans : -1;
    }
};
```

> 灵茶山艾府：https://leetcode.cn/problems/design-graph-with-shortest-path-calculator/solutions/2229013/dijkstra-suan-fa-mo-ban-pythonjavacgo-by-unmv/