

# No.113 Biweekly competition

## Easy 8039. 使数组成为递增数组的最少右移次数 3 points

给你一个长度为 `n` 下标从 **0** 开始的数组 `nums` ，数组中的元素为 **互不相同** 的正整数。请你返回让 `nums` 成为递增数组的 **最少右移** 次数，如果无法得到递增数组，返回 `-1` 。

一次 **右移** 指的是同时对所有下标进行操作，将下标为 `i` 的元素移动到下标 `(i + 1) % n` 处。

 

**示例 1：**

```
输入：nums = [3,4,5,1,2]
输出：2
解释：
第一次右移后，nums = [2,3,4,5,1] 。
第二次右移后，nums = [1,2,3,4,5] 。
现在 nums 是递增数组了，所以答案为 2 。
```

**示例 2：**

```
输入：nums = [1,3,5]
输出：0
解释：nums 已经是递增数组了，所以答案为 0 。
```

**示例 3：**

```
输入：nums = [2,1,4]
输出：-1
解释：无法将数组变为递增数组。
```

 

**提示：**

- `1 <= nums.length <= 100`
- `1 <= nums[i] <= 100`
- `nums` 中的整数互不相同。



### 暴力

使用暴力的方法，在遍历的过程中应该只能找到最多两段递增的序列，故而在遍历中进行记录并判断中间断点的位置，并进行判断。

```cpp
class Solution {
public:
    int minimumRightShifts(vector<int>& nums) {
        
        int n = nums.size();
        int pos = -1; // 记录断点
        int flag = 0;
            
        for(int i =0 ; i < n-1 ;i++)
        {        
        if(flag == 0) // 还未出现断点
        {
            if(nums[i] > nums[i+1])
            {
                pos = i;
                flag = 1;
            }
        }
        else// 出现了断点
        {
            if(nums[i] > nums[i+1] || nums[i] > nums[pos]) return -1; // 再次出现断点
        }
        }
        if(pos == -1) return 0;// 没有出现断点，一直在递增
        if(nums[n-1] > nums[pos]) return -1;// 无法满足转移后递增    
        
        return n-pos-1;
    }
};
```

---

## Medium 2856. 删除数对后的最小数组长度 4 points

给你一个下标从 **0** 开始的 **非递减** 整数数组 `nums` 。

你可以执行以下操作任意次：

- 选择 **两个** 下标 `i` 和 `j` ，满足 `i < j` 且 `nums[i] < nums[j]` 。
- 将 `nums` 中下标在 `i` 和 `j` 处的元素删除。剩余元素按照原来的顺序组成新的数组，下标也重新从 **0** 开始编号。

请你返回一个整数，表示执行以上操作任意次后（可以执行 **0** 次），`nums` 数组的 **最小** 数组长度。

 

**示例 1：**

```
输入：nums = [1,3,4,9]
输出：0
解释：一开始，nums = [1, 3, 4, 9] 。
第一次操作，我们选择下标 0 和 1 ，满足 nums[0] < nums[1] <=> 1 < 3 。
删除下标 0 和 1 处的元素，nums 变成 [4, 9] 。
下一次操作，我们选择下标 0 和 1 ，满足 nums[0] < nums[1] <=> 4 < 9 。
删除下标 0 和 1 处的元素，nums 变成空数组 [] 。
所以，可以得到的最小数组长度为 0 。
```

**示例 2：**

```
输入：nums = [2,3,6,9]
输出：0
解释：一开始，nums = [2, 3, 6, 9] 。
第一次操作，我们选择下标 0 和 2 ，满足 nums[0] < nums[2] <=> 2 < 6 。
删除下标 0 和 2 处的元素，nums 变成 [3, 9] 。
下一次操作，我们选择下标 0 和 1 ，满足 nums[0] < nums[1] <=> 3 < 9 。
删除下标 0 和 1 处的元素，nums 变成空数组 [] 。
所以，可以得到的最小数组长度为 0 。
```

**示例 3：**

```
输入：nums = [1,1,2]
输出：1
解释：一开始，nums = [1, 1, 2] 。
第一次操作，我们选择下标 0 和 2 ，满足 nums[0] < nums[2] <=> 1 < 2 。
删除下标 0 和 2 处的元素，nums 变成 [1] 。
无法对数组再执行操作。
所以，可以得到的最小数组长度为 1 。
```

 

**提示：**

- `1 <= nums.length <= 105`
- `1 <= nums[i] <= 109`
- `nums` 是 **非递减** 数组。



### 贪心

很巧妙的题，正确的消除方法是，找到出现次数最多的那个数x，其他的数字和它消除，只会出现以下两种情况：

1. `x >= n/2`，则无法全部消除，会剩下`x - (n - x)  -> 2 * x - n`
2. `x < n/2`，则由其他的自行消除到 x 个（或与x相差1个），然后消除x，剩下的数字的数量取决于总数的奇偶性。

```cpp
class Solution {
public:
    int minLengthAfterRemovals(vector<int>& nums) {
        int n = nums.size();

        int cnt = 0;
        for(int i = 0; i < n; )
        {
            int temp = 1;
            int pos = i;
            while(++i<n)
            {
                if(nums[i] != nums[i-1]) break;
            }
            temp = i - pos;
            cnt = max(cnt , temp); // 获取出现次数最多的数字的数量
        }
        return max(cnt * 2 - n, n % 2);
    }
};
```



### 二分

刚刚通过遍历的方法来找到出现次数最多的那个数。

如果存在这么一个数，其出现次数 > n/2（这个时候它就一定是出现次数最多的那个数），那么它一定在【n/2】的索引位置。

找到这个数后，通过二分法来确定这个数的个数，然后再次使用上面的判断方法。

```cpp
class Solution {
public:
    int minLengthAfterRemovals(vector<int> &nums) {
        int n = nums.size();
        int x = nums[n / 2];
        int max_cnt = upper_bound(nums.begin(), nums.end(), x) -
                      lower_bound(nums.begin(), nums.end(), x);
        // 找到大于x的第一个指针和大于等于x的第一个指针
        return max(max_cnt * 2 - n, n % 2);
    }
};
```



---

## Medium 6988. 统计距离为 k 的点对 5 points

给你一个 **二维** 整数数组 `coordinates` 和一个整数 `k` ，其中 `coordinates[i] = [xi, yi]` 是第 `i` 个点在二维平面里的坐标。

我们定义两个点 `(x1, y1)` 和 `(x2, y2)` 的 **距离** 为 `(x1 XOR x2) + (y1 XOR y2)` ，`XOR` 指的是按位异或运算。

请你返回满足 `i < j` 且点 `i` 和点 `j`之间距离为 `k` 的点对数目。

 

**示例 1：**

```
输入：coordinates = [[1,2],[4,2],[1,3],[5,2]], k = 5
输出：2
解释：以下点对距离为 k ：
- (0, 1)：(1 XOR 4) + (2 XOR 2) = 5 。
- (2, 3)：(1 XOR 5) + (3 XOR 2) = 5 。
```

**示例 2：**

```
输入：coordinates = [[1,3],[1,3],[1,3],[1,3],[1,3]], k = 0
输出：10
解释：任何两个点之间的距离都为 0 ，所以总共有 10 组点对。
```

 

**提示：**

- `2 <= coordinates.length <= 50000`
- `0 <= xi, yi <= 106`
- `0 <= k <= 100`



### 哈希表

要求`(x1 XOR x2) + (y1 XOR y2) == k`，则是：

`(x1 XOR x2) == i , (y1 XOR y2) == k-i`，两边同时异或可以得到：

`x1 == i ^ x2  ,  y1 == (k-i) ^ y2`，使用哈希表记录并计数。

```cpp
class Solution {
public:
    int countPairs(vector<vector<int>>& coordinates, int k) {
        unordered_map<long long,int> cnt;
        int ans = 0;
        
        for(auto point:coordinates)
        {
            int x = point[0];
            int y = point[1];
            for(int i = 0;i <= k; i++)
            {
                ans += cnt[(x^i) *1e6 + (y^(k-i))];
            }
            cnt[x*1e6+y]++;
        }
        return ans;
    }
};
```



---

## Hard 100041. 可以到达每一个节点的最少边反转次数 6 points

给你一个 `n` 个点的 **简单有向图** （没有重复边的有向图），节点编号为 `0` 到 `n - 1` 。如果这些边是双向边，那么这个图形成一棵 **树** 。

给你一个整数 `n` 和一个 **二维** 整数数组 `edges` ，其中 `edges[i] = [ui, vi]` 表示从节点 `ui` 到节点 `vi` 有一条 **有向边** 。

**边反转** 指的是将一条边的方向反转，也就是说一条从节点 `ui` 到节点 `vi` 的边会变为一条从节点 `vi` 到节点 `ui` 的边。

对于范围 `[0, n - 1]` 中的每一个节点 `i` ，你的任务是分别 **独立** 计算 **最少** 需要多少次 **边反转** ，从节点 `i` 出发经过 **一系列有向边** ，可以到达所有的节点。

请你返回一个长度为 `n` 的整数数组 `answer` ，其中 `answer[i]`表示从节点 `i` 出发，可以到达所有节点的 **最少边反转** 次数。

 

**示例 1：**

![img](https://assets.leetcode.com/uploads/2023/08/26/image-20230826221104-3.png)

```
输入：n = 4, edges = [[2,0],[2,1],[1,3]]
输出：[1,1,0,2]
解释：上图表示了与输入对应的简单有向图。
对于节点 0 ：反转 [2,0] ，从节点 0 出发可以到达所有节点。
所以 answer[0] = 1 。
对于节点 1 ：反转 [2,1] ，从节点 1 出发可以到达所有节点。
所以 answer[1] = 1 。
对于节点 2 ：不需要反转就可以从节点 2 出发到达所有节点。
所以 answer[2] = 0 。
对于节点 3 ：反转 [1,3] 和 [2,1] ，从节点 3 出发可以到达所有节点。
所以 answer[3] = 2 。
```

**示例 2：**

![img](https://assets.leetcode.com/uploads/2023/08/26/image-20230826225541-2.png)

```
输入：n = 3, edges = [[1,2],[2,0]]
输出：[2,0,1]
解释：上图表示了与输入对应的简单有向图。
对于节点 0 ：反转 [2,0] 和 [1,2] ，从节点 0 出发可以到达所有节点。
所以 answer[0] = 2 。
对于节点 1 ：不需要反转就可以从节点 2 出发到达所有节点。
所以 answer[1] = 0 。
对于节点 2 ：反转 [1,2] ，从节点 2 出发可以到达所有节点。
所以 answer[2] = 1 。
```

 

**提示：**

- `2 <= n <= 105`
- `edges.length == n - 1`
- `edges[i].length == 2`
- `0 <= ui == edges[i][0] < n`
- `0 <= vi == edges[i][1] < n`
- `ui != vi`
- 输入保证如果边是双向边，可以得到一棵树。



### 换根DP

```cpp
class Solution {
public:
    vector<int> minEdgeReversals(int n, vector<vector<int>> &edges) {
        
        /* 建图 */
        vector<vector<pair<int, int>>> g(n);
        // 第一层维度是x , 第二层维度是y , 第三层维度是方向正反
        // 表明 x->y 是正还是反 ， y->x 是正还是反
        for (auto &e: edges) {
            int x = e[0], y = e[1];
            g[x].emplace_back(y, 1);
            g[y].emplace_back(x, -1); // 从 y 到 x 需要反向
        }

        vector<int> ans(n, 0);
        function<void(int, int)> dfs = [&](int x, int fa) {
            // 对x结点连接的所有的结点进行判断
            for (auto &[y, dir]: g[x]) {
                if (y != fa) { // 排除此前的结点
                    ans[0] += dir < 0; // 如果是正向的，则不需要加入数量
                    dfs(y, x); // 以另一个结点继续计算，不需要计算当前节点
                }
            }
        };
        dfs(0, -1);

        // 换根计算
        function<void(int, int)> reroot = [&](int x, int fa) {
            for (auto &[y, dir]: g[x]) {
                if (y != fa) {
                    ans[y] = ans[x] + dir; // dir 就是从 x 换到 y 的「变化量」
                    reroot(y, x);
                }
            }
        };
        reroot(0, -1);
        return ans;
    }
};

```

> 详细参考灵茶山艾府：https://leetcode.cn/problems/minimum-edge-reversals-so-every-node-is-reachable/solutions/2445681/mo-ban-huan-gen-dppythonjavacgojs-by-end-8qiu/

