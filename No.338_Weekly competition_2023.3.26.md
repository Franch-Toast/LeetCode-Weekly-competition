# No.338 Weekly competition

## Easy 2600. K 件物品的最大和 3 points

袋子中装有一些物品，每个物品上都标记着数字 `1` 、`0` 或 `-1` 。

给你四个非负整数 `numOnes` 、`numZeros` 、`numNegOnes` 和 `k` 。

袋子最初包含：

- `numOnes` 件标记为 `1` 的物品。
- `numZeroes` 件标记为 `0` 的物品。
- `numNegOnes` 件标记为 `-1` 的物品。

现计划从这些物品中恰好选出 `k` 件物品。返回所有可行方案中，物品上所标记数字之和的最大值。

 

**示例 1：**

```
输入：numOnes = 3, numZeros = 2, numNegOnes = 0, k = 2
输出：2
解释：袋子中的物品分别标记为 {1, 1, 1, 0, 0} 。取 2 件标记为 1 的物品，得到的数字之和为 2 。
可以证明 2 是所有可行方案中的最大值。
```

**示例 2：**

```
输入：numOnes = 3, numZeros = 2, numNegOnes = 0, k = 4
输出：3
解释：袋子中的物品分别标记为 {1, 1, 1, 0, 0} 。取 3 件标记为 1 的物品，1 件标记为 0 的物品，得到的数字之和为 3 。
可以证明 3 是所有可行方案中的最大值。
```

 

**提示：**

- `0 <= numOnes, numZeros, numNegOnes <= 50`
- `0 <= k <= numOnes + numZeros + numNegOnes`



### 分类讨论

```cpp
class Solution {
public:
    int kItemsWithMaximumSum(int numOnes, int numZeros, int numNegOnes, int k) {
        if(k<=numOnes) return k;
        else if(k<=numZeros+numOnes) return numOnes;
        else return numOnes-(k-numOnes-numZeros);
        
    }
};
```



---

## Medium 2601. 质数减法运算 4 points

给你一个下标从 **0** 开始的整数数组 `nums` ，数组长度为 `n` 。

你可以执行无限次下述运算：

- 选择一个之前未选过的下标 `i` ，并选择一个 **严格小于** `nums[i]` 的质数 `p` ，从 `nums[i]` 中减去 `p` 。

如果你能通过上述运算使得 `nums` 成为严格递增数组，则返回 `true` ；否则返回 `false` 。

**严格递增数组** 中的每个元素都严格大于其前面的元素。

 

**示例 1：**

```
输入：nums = [4,9,6,10]
输出：true
解释：
在第一次运算中：选择 i = 0 和 p = 3 ，然后从 nums[0] 减去 3 ，nums 变为 [1,9,6,10] 。
在第二次运算中：选择 i = 1 和 p = 7 ，然后从 nums[1] 减去 7 ，nums 变为 [1,2,6,10] 。
第二次运算后，nums 按严格递增顺序排序，因此答案为 true 。
```

**示例 2：**

```
输入：nums = [6,8,11,12]
输出：true
解释：nums 从一开始就按严格递增顺序排序，因此不需要执行任何运算。
```

**示例 3：**

```
输入：nums = [5,8,3]
输出：false
解释：可以证明，执行运算无法使 nums 按严格递增顺序排序，因此答案是 false 。
```

 

**提示：**

- `1 <= nums.length <= 1000`
- `1 <= nums[i] <= 1000`
- `nums.length == n`



### 质数/模拟

思路是从后向前，因为题目中表示的操作只能减去质数而不能增加质数，所以从后向前来遍历比较合理。一旦发现不严格递增的项，则在[ nums[i]-nums[i+1]+1 , nums[i] )范围内找到最小的质数，尽量给前面的数留空间。

```cpp
class Solution {
public:
    bool primeSubOperation(vector<int>& nums) {
        for(int i=nums.size()-2;i>=0;i--)
        {
            if(nums[i]>=nums[i+1])//不递增
            {
                //查找是否有满足要求的最小质数
                for(int j=nums[i]-nums[i+1]+1;j<nums[i];j++)
                {
                    bool flag=1;
                    for(int k=2;k<j;k++)
                    {
                        if(j%k==0) //j是合数
                        {
                            flag=0;
                            break;
                        }
                    }
                    if(flag && j!=1) //j是满足要求的最小质数，nums减去它
                    {
                        nums[i]-=j;
                        break;
                    }
                    if(j==nums[i]-1) return 0;//如果查询到nums[i]-1仍然找不到相应的质数，则说明不能找到质数使其递减
                }
                if(nums[i]>=nums[i+1]) return 0;
            }
        }
        return 1;
    }
};
```

### 筛质数/二分

灵神使用的方法，先预先筛选出来0-1000以内的质数并存在表中，在从头向后进行遍历判断，对不严格递增的项剪掉尽可能大的质数（二分法来查找表内预先存好的质数），为后面的项预留空间。这里筛质数的方法非常不错（欧拉筛），值得学习思路。

```cpp
const int MX = 1000;
vector<int> primes{0}; // 哨兵，避免二分越界

int init = []() {
    bool np[MX]{};
    for (int i = 2; i < MX; ++i)
        if (!np[i]) {
            primes.push_back(i); // 预处理质数列表
            for (int j = i; j < MX / i; ++j)
                np[i * j] = true;
        }
    return 0;
}();

class Solution {
public:
    bool primeSubOperation(vector<int> &nums) {
        int pre = 0;
        for (int x: nums) {
            if (x <= pre) return false;
            pre = x - *--lower_bound(primes.begin(), primes.end(), x - pre);
        }
        return true;
    }
};

```



---

## Medium 2602. 使数组元素全部相等的最少操作次数 5 points

给你一个正整数数组 `nums` 。

同时给你一个长度为 `m` 的整数数组 `queries` 。第 `i` 个查询中，你需要将 `nums` 中所有元素变成 `queries[i]` 。你可以执行以下操作 **任意** 次：

- 将数组里一个元素 **增大** 或者 **减小** `1` 。

请你返回一个长度为 `m` 的数组 `answer` ，其中 `answer[i]`是将 `nums` 中所有元素变成 `queries[i]` 的 **最少** 操作次数。

**注意**，每次查询后，数组变回最开始的值。

 

**示例 1：**

```
输入：nums = [3,1,6,8], queries = [1,5]
输出：[14,10]
解释：第一个查询，我们可以执行以下操作：
- 将 nums[0] 减小 2 次，nums = [1,1,6,8] 。
- 将 nums[2] 减小 5 次，nums = [1,1,1,8] 。
- 将 nums[3] 减小 7 次，nums = [1,1,1,1] 。
第一个查询的总操作次数为 2 + 5 + 7 = 14 。
第二个查询，我们可以执行以下操作：
- 将 nums[0] 增大 2 次，nums = [5,1,6,8] 。
- 将 nums[1] 增大 4 次，nums = [5,5,6,8] 。
- 将 nums[2] 减小 1 次，nums = [5,5,5,8] 。
- 将 nums[3] 减小 3 次，nums = [5,5,5,5] 。
第二个查询的总操作次数为 2 + 4 + 1 + 3 = 10 。
```

**示例 2：**

```
输入：nums = [2,9,6,3], queries = [10]
输出：[20]
解释：我们可以将数组中所有元素都增大到 10 ，总操作次数为 8 + 1 + 4 + 7 = 20 。
```

 

**提示：**

- `n == nums.length`
- `m == queries.length`
- `1 <= n, m <= 105`
- `1 <= nums[i], queries[i] <= 109`

### 排序+前缀和+二分

排序后计算前缀和，根据二分法找到元素与target的大小分界点，再根据前后的元素数量进行计算：

假设目标值为 target ，在排序后前 i 个数 <= target ，而后 j 个数 > target ，则

`ans=i*target - (x1+x2+...+xi) + (xi+1+xi+2+...+xm) - j*target`

```cpp
class Solution {
public:
    vector<long long> minOperations(vector<int>& nums, vector<int>& queries) {
        //计算前缀和，再使用二分法找到正负号变换点
        vector<long long> ans;
        vector<long long> pre(nums.size()+1);
        
        sort(nums.begin(),nums.end());
        
        for(int i=0;i<nums.size();i++)//计算前缀和
        {
            pre[i+1]=pre[i]+nums[i];
        }
        
        for(int i=0;i<queries.size();i++)
        {
            int left=0;
            int right=nums.size()-1;
            while(left<=right)//二分法
            {
                int mid=(right-left)/2+left;
                if(nums[mid]>=queries[i]) right=mid-1;
                else left=mid+1;
            }//right索引的数>=queries[i]
            ans.push_back(*(pre.end()-1)-2*pre[right+1]+(2*(right+1)-nums.size())*queries[i]);
        }
        
        return ans;
    }
};
```

灵神给出的同样思路的做法：

<img src="https://pic.leetcode.cn/1679808210-FVsAou-t3.png" alt="t3.png" style="zoom:33%;" />

```cpp
class Solution {
public:
    vector<long long> minOperations(vector<int> &nums, vector<int> &queries) {
        sort(nums.begin(), nums.end());
        int n = nums.size();
        long long sum[n + 1]; // 前缀和
        sum[0] = 0;
        for (int i = 0; i < n; ++i)
            sum[i + 1] = sum[i] + nums[i];

        int m = queries.size();
        vector<long long> ans(m);
        for (int i = 0; i < m; ++i) {
            int q = queries[i];
            long long j = lower_bound(nums.begin(), nums.end(), q) - nums.begin();
            long long left = q * j - sum[j]; // 蓝色面积
            long long right = sum[n] - sum[j] - q * (n - j); // 绿色面积
            ans[i] = left + right;
        }
        return ans;
    }
};
```



---

## Hard 2603. 收集树中金币 MEX 6 points

给你一个 `n` 个节点的无向无根树，节点编号从 `0` 到 `n - 1` 。给你整数 `n` 和一个长度为 `n - 1` 的二维整数数组 `edges` ，其中 `edges[i] = [ai, bi]` 表示树中节点 `ai` 和 `bi` 之间有一条边。再给你一个长度为 `n` 的数组 `coins` ，其中 `coins[i]` 可能为 `0` 也可能为 `1` ，`1` 表示节点 `i` 处有一个金币。

一开始，你需要选择树中任意一个节点出发。你可以执行下述操作任意次：

- 收集距离当前节点距离为 `2` 以内的所有金币，或者
- 移动到树中一个相邻节点。

你需要收集树中所有的金币，并且回到出发节点，请你返回最少经过的边数。

如果你多次经过一条边，每一次经过都会给答案加一。

 

**示例 1：**

![img](https://assets.leetcode.com/uploads/2023/03/01/graph-2.png)

```
输入：coins = [1,0,0,0,0,1], edges = [[0,1],[1,2],[2,3],[3,4],[4,5]]
输出：2
解释：从节点 2 出发，收集节点 0 处的金币，移动到节点 3 ，收集节点 5 处的金币，然后移动回节点 2 。
```

**示例 2：**

![img](https://assets.leetcode.com/uploads/2023/03/02/graph-4.png)

```
输入：coins = [0,0,0,1,1,0,0,1], edges = [[0,1],[0,2],[1,3],[1,4],[2,5],[5,6],[5,7]]
输出：2
解释：从节点 0 出发，收集节点 4 和 3 处的金币，移动到节点 2 处，收集节点 7 处的金币，移动回节点 0 。
```

 

**提示：**

- `n == coins.length`
- `1 <= n <= 3 * 104`
- `0 <= coins[i] <= 1`
- `edges.length == n - 1`
- `edges[i].length == 2`
- `0 <= ai, bi < n`
- `ai != bi`
- `edges` 表示一棵合法的树。



### 拓扑排序

```cpp
class Solution {
public:
    int collectTheCoins(vector<int> &coins, vector<vector<int>> &edges) {
        int n = coins.size();
        vector<vector<int>> g(n);//用于存放与索引相连的结点
        int deg[n]; //用于记录与结点相连接的结点的数量
        memset(deg, 0, sizeof(deg));//将deg全部填充为 0 
        for (auto &e: edges) {
            int x = e[0], y = e[1];
            g[x].push_back(y);
            g[y].push_back(x); // 建图
            ++deg[x];
            ++deg[y];
        }

        // 用拓扑排序「剪枝」：去掉没有金币的子树
        queue<int> q;//用来存放待处理结点
        for (int i = 0; i < n; ++i)
            if (deg[i] == 1 && coins[i] == 0) // 无金币，且是叶子（仅有一条边连接）
                q.push(i);
        while (!q.empty()) {//处理结点，因为删除这个结点意味着要把与他相连的结点的结点连接数--
            int x = q.front();
            q.pop();
            for (int y: g[x])//遍历与 x 相连的结点
                if (--deg[y] == 1 && coins[y] == 0)
                    //如果删除叶子节点后该节点已经成为叶子结点且是无金币的，则加入队列等待被删除
                    q.push(y);
        }

        // 经过上面的操作后，目前所有的叶子节点都是有金币的。再次拓扑排序
        for (int i = 0; i < n; ++i)
            if (deg[i] == 1 && coins[i]) // 有金币叶子
                q.push(i);
        if (q.size() <= 1) return 0; // 至多只有一个有金币的叶子，直接收集
        int time[n]; 
        memset(time, 0, sizeof(time));
        while (!q.empty()) {
            int x = q.front();
            q.pop();
            for (int y: g[x])
                if (--deg[y] == 1) {//当结点变为叶子结点时入队待处理，知道所有的结点都被遍历
                    time[y] = time[x] + 1; // 记录入队时间，比它的前驱结点晚一个时刻入队
                    q.push(y);
                }
        }

        // 统计答案
        int ans = 0;
        for (auto &e: edges)
            if (time[e[0]] >= 2 && time[e[1]] >= 2)//当连接点的两端都经过了至少两轮以上的拓扑排序，说明这条边一定要走两次
                ans += 2;
        return ans;
    }
};
```

实在是非常聪明的做法，通过多次的拓扑排序以及逻辑思考很清晰地将这个题做出来，适合多次思考。

> 灵茶山艾府：https://leetcode.cn/problems/collect-coins-in-a-tree/solutions/2191371/tuo-bu-pai-xu-ji-lu-ru-dui-shi-jian-pyth-6uli/