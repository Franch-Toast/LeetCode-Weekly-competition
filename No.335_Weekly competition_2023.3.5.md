# No.335 Weekly competition

## Easy 2582. 递枕头 3 points

- `n` 个人站成一排，按从 `1` 到 `n` 编号。

  最初，排在队首的第一个人拿着一个枕头。每秒钟，拿着枕头的人会将枕头传递给队伍中的下一个人。一旦枕头到达队首或队尾，传递方向就会改变，队伍会继续沿相反方向传递枕头。

  - 例如，当枕头到达第 `n` 个人时，TA 会将枕头传递给第 `n - 1` 个人，然后传递给第 `n - 2` 个人，依此类推。

  给你两个正整数 `n` 和 `time` ，返回 `time` 秒后拿着枕头的人的编号。

   

  **示例 1：**

  ```
  输入：n = 4, time = 5
  输出：2
  解释：队伍中枕头的传递情况为：1 -> 2 -> 3 -> 4 -> 3 -> 2 。
  5 秒后，枕头传递到第 2 个人手中。
  ```

  **示例 2：**

  ```
  输入：n = 3, time = 2
  输出：3
  解释：队伍中枕头的传递情况为：1 -> 2 -> 3 。
  2 秒后，枕头传递到第 3 个人手中。
  ```

   

  **提示：**

  - `2 <= n <= 1000`
  - `1 <= time <= 1000`

### 数学

```cpp
class Solution {
public:
    int passThePillow(int n, int time) {
        
        if(time/(n-1)%2==1)//向回传
        {
            return n-time%(n-1);
        }
        else//向前传
        {
            return 1+time%(n-1);
        }
    }
};
```



---

## Medium 2583. 二叉树中的第 K 大层和 4 points

- 给你一棵二叉树的根节点 `root` 和一个正整数 `k` 。

  树中的 **层和** 是指 **同一层** 上节点值的总和。

  返回树中第 `k` 大的层和（不一定不同）。如果树少于 `k` 层，则返回 `-1` 。

  **注意**，如果两个节点与根节点的距离相同，则认为它们在同一层。

   

  **示例 1：**

  ![img](https://assets.leetcode.com/uploads/2022/12/14/binaryytreeedrawio-2.png)

  ```
  输入：root = [5,8,9,2,1,3,7,4,6], k = 2
  输出：13
  解释：树中每一层的层和分别是：
  - Level 1: 5
  - Level 2: 8 + 9 = 17
  - Level 3: 2 + 1 + 3 + 7 = 13
  - Level 4: 4 + 6 = 10
  第 2 大的层和等于 13 。
  ```

  **示例 2：**

  ![img](https://assets.leetcode.com/uploads/2022/12/14/treedrawio-3.png)

  ```
  输入：root = [1,2,null,3], k = 1
  输出：3
  解释：最大的层和是 3 。
  ```

   

  **提示：**

  - 树中的节点数为 `n`
  - `2 <= n <= 105`
  - `1 <= Node.val <= 106`
  - `1 <= k <= n`





### BFS广度优先遍历

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
    long long kthLargestLevelSum(TreeNode* root, int k) {
        
        
        queue<TreeNode*> q;//用来存放节点
        q.push(root);
        
        priority_queue<long long> pq;//用来存放层数和
        
        
        
        while(!q.empty())
        {
            int curcount=q.size();
            long long sum=0;
            for(int i=0;i<curcount;i++)
            {
                auto node=q.front();
                q.pop();
                sum+=node->val;
                if(node->left!=NULL) q.push(node->left);
                if(node->right!=NULL) q.push(node->right);
            }
            pq.push(sum);
        
        }
        if(pq.size()<k) return -1;
        else 
        {
            for(int i=0;i<k-1;i++) pq.pop();
            return pq.top();
        }
        
    }
};
```



---

## Hard 2584. 分割数组使乘积互质 5 points

给你一个长度为 `n` 的整数数组 `nums` ，下标从 **0** 开始。

如果在下标 `i` 处 **分割** 数组，其中 `0 <= i <= n - 2` ，使前 `i + 1` 个元素的乘积和剩余元素的乘积互质，则认为该分割 **有效** 。

- 例如，如果 `nums = [2, 3, 3]` ，那么在下标 `i = 0` 处的分割有效，因为 `2` 和 `9` 互质，而在下标 `i = 1` 处的分割无效，因为 `6` 和 `3` 不互质。在下标 `i = 2` 处的分割也无效，因为 `i == n - 1` 。

返回可以有效分割数组的最小下标 `i` ，如果不存在有效分割，则返回 `-1` 。

当且仅当 `gcd(val1, val2) == 1` 成立时，`val1` 和 `val2` 这两个值才是互质的，其中 `gcd(val1, val2)` 表示 `val1` 和 `val2` 的最大公约数。

 

**示例 1：**

![img](https://assets.leetcode.com/uploads/2022/12/14/second.PNG)

```
输入：nums = [4,7,8,15,3,5]
输出：2
解释：上表展示了每个下标 i 处的前 i + 1 个元素的乘积、剩余元素的乘积和它们的最大公约数的值。
唯一一个有效分割位于下标 2 。
```

**示例 2：**

![img](https://assets.leetcode.com/uploads/2022/12/14/capture.PNG)

```
输入：nums = [4,7,15,8,3,5]
输出：-1
解释：上表展示了每个下标 i 处的前 i + 1 个元素的乘积、剩余元素的乘积和它们的最大公约数的值。
不存在有效分割。
```

 

**提示：**

- `n == nums.length`
- `1 <= n <= 104`
- `1 <= nums[i] <= 106`



### 分解质因数

思路如下：

**两个部分互质：即左半部分和右半部分，没有公共的质因子。**
那么可以来思考哪些地方不能分割。如果我求出每个元素的质因子，并将其存入表内，那么，
对每个质因子，都处理它在 nums 中的最左边的下标和最右边的下标 left 和 right ，
那么答案不能在 [left, right) 中  => 最小的答案可能是 right 。
**最后的答案即为：$max(right_i)$，即所有质因子的最右边下标的最大值。**

```cpp
class Solution {
public:
    int findValidSplit(vector<int> &nums) {
        unordered_map<int, int> left; // left[p] 表示质数 p 首次出现的下标
        int n = nums.size(), right[n]; // right[i] 表示左端点为 i 的区间的右端点的最大值
        memset(right, 0, sizeof(right));
        auto f = [&](int p, int i) {
            auto it = left.find(p);
            if (it == left.end())
                left[p] = i; // 第一次遇到质数 p ，记录该质因数出现第一次的位置是 i 
            else
                right[it->second] = i; // 记录左端点 l 对应的右端点，这个值会不断更新变大，因为传入的i的值会不断变大
        };

        for (int i = 0; i < n; ++i) {
            int x = nums[i];//对 x 进行分解质因数
            for (int d = 2; d * d <= x; ++d) { // 从 2 开始一直到 sqrt(x)
                if (x % d == 0) {//如果是质因数
                    f(d, i);
                    for (x /= d; x % d == 0; x /= d);//将该质因数完全除尽，开始寻找下一个质因数
                }
            }
            if (x > 1) f(x, i);//将元素本身作为一个因数，这一点容易遗忘
        }

        for (int l = 0, max_r = 0; l < n; ++l) {
            if (l > max_r) // 最远可以遇到 max_r
                return max_r; // 也可以写 l-1
            max_r = max(max_r, right[l]);
        }
        return -1;
    }
};
```



---

## Hard 2585. 获得分数的方法数 6 points

考试中有 `n` 种类型的题目。给你一个整数 `target` 和一个下标从 **0** 开始的二维整数数组 `types` ，其中 `types[i] = [counti, marksi] `表示第 `i` 种类型的题目有 `counti` 道，每道题目对应 `marksi` 分。

返回你在考试中恰好得到 `target` 分的方法数。由于答案可能很大，结果需要对 `109 +7` 取余。

**注意**，同类型题目无法区分。

- 比如说，如果有 `3` 道同类型题目，那么解答第 `1` 和第 `2` 道题目与解答第 `1` 和第 `3` 道题目或者第 `2` 和第 `3` 道题目是相同的。

 

**示例 1：**

```
输入：target = 6, types = [[6,1],[3,2],[2,3]]
输出：7
解释：要获得 6 分，你可以选择以下七种方法之一：
- 解决 6 道第 0 种类型的题目：1 + 1 + 1 + 1 + 1 + 1 = 6
- 解决 4 道第 0 种类型的题目和 1 道第 1 种类型的题目：1 + 1 + 1 + 1 + 2 = 6
- 解决 2 道第 0 种类型的题目和 2 道第 1 种类型的题目：1 + 1 + 2 + 2 = 6
- 解决 3 道第 0 种类型的题目和 1 道第 2 种类型的题目：1 + 1 + 1 + 3 = 6
- 解决 1 道第 0 种类型的题目、1 道第 1 种类型的题目和 1 道第 2 种类型的题目：1 + 2 + 3 = 6
- 解决 3 道第 1 种类型的题目：2 + 2 + 2 = 6
- 解决 2 道第 2 种类型的题目：3 + 3 = 6
```

**示例 2：**

```
输入：target = 5, types = [[50,1],[50,2],[50,5]]
输出：4
解释：要获得 5 分，你可以选择以下四种方法之一：
- 解决 5 道第 0 种类型的题目：1 + 1 + 1 + 1 + 1 = 5
- 解决 3 道第 0 种类型的题目和 1 道第 1 种类型的题目：1 + 1 + 1 + 2 = 5
- 解决 1 道第 0 种类型的题目和 2 道第 1 种类型的题目：1 + 2 + 2 = 5
- 解决 1 道第 2 种类型的题目：5
```

**示例 3：**

```
输入：target = 18, types = [[6,1],[3,2],[2,3]]
输出：1
解释：只有回答所有题目才能获得 18 分。
```

 

**提示：**

- `1 <= target <= 1000`
- `n == types.length`
- `1 <= n <= 50`
- `types[i].length == 2`
- `1 <= counti, marksi <= 50`



### DP

这题可以通过动态规划的思路分析为以下问题：

原问题: n种题目，恰好组合`target`分的方案数
假设最后一种题目，做了`k`道题
子问题: `n-1` 种题目，恰好组合`target- types[n-1][1]*k`分的方案数

下面的代码是通过递推的方式来进行计算的结果：

```cpp
class Solution {
    const int MOD=1e9+7;
public:
    int waysToReachTarget(int target, vector<vector<int>>& types) {
        return dfs(types.size()-1,target,types)%MOD;

    }
    int dfs(int i,int target,vector<vector<int>>& types)
    {
        if(i<0)  return target==0?1:0; //递归到头了，判断到第0道题后目标值是否是0
        // k*types[i][1]<=target
        // k<=target/target[i][1]
        int res=0;
        for(int k=0;k<=min(target/types[i][1],types[i][0]);k++)//从做 0 题开始一直计算直到 min(所有题，回溯点)
        {
            res+=dfs(i-1,target-types[i][1]*k,types);
        }
        return res;
    }
};
```

这里需要注意的是一定要加入记忆化搜索，不然就会超时，故而下面的代码使用 记忆化搜索 + 迭代 取代了上面的递推方法。



**记忆化搜索 + 迭代 状态转移递推思路：**

`f[前i题组合][target]=f[前i-1题组合][target](i题不做，方案数与同目标分i-1题组合一致)+f[前i-1题组合][target-k*type[i][1]](i题做k道，方案数是i-1题组合，目标分是当前目标分减去做了i题k道的分数)`
这里 k 需要进行遍历，因为要找出所有的方案数。k 在遍历到超过目标分时就可以 break 回溯了。

```cpp
class Solution {
public:
    int waysToReachTarget(int target, vector<vector<int>>& types) {
        int n = types.size(), mod =1e9 + 7;
        vector<vector<int>>f(n + 1, vector<int>(target + 1));
        for(int i = 0; i <= n; i++)f[i][0] = 1; // 前 i 种题目，做题达到分数恰好为 0 的情况只有一种，即一题都不做
        for(int i = 1; i <= n; i++){//从前向后迭代计算
            for(int j = 1; j <= target; j++){
                f[i][j] = f[i - 1][j]; // 因为目标分没变，所以可以不做第i种题型，直接将前面的方案数赋值过来
                auto v = types[i - 1];
                for(int k = 1; k <= v[0]; k++){ // 做第i种题型,做k题
                    if(v[1] * k > j) break; // 单单做k题得分就超过目标得分j了,不合理break.
                    f[i][j] += f[i - 1][j - v[1] * k]; // 由前边得分j - v[1] * k转移而来.
                    f[i][j] %= mod;
                }
            }
        }
        return f[n][target];
    }
};
```

灵神给出了从后向前的思路，这样可以将二维记忆数组优化为一维记忆数组，这里没有做详细解释：

```cpp
class Solution {
    const int MOD = 1e9 + 7;
public:
    int waysToReachTarget(int target, vector<vector<int>> &types) {
        int f[target + 1];
        memset(f, 0, sizeof(f));
        f[0] = 1;
        for (auto &p : types) {
            int count = p[0], marks = p[1];
            for (int j = target; j > 0; --j)
                for (int k = 1; k <= min(count, j / marks); ++k)
                    f[j] = (f[j] + f[j - k * marks]) % MOD;
        }
        return f[target];
    }
};
```

