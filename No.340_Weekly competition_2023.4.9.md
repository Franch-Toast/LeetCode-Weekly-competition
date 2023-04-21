# No.340 Weekly competition

## Easy 2614. 对角线上的质数 3 points

给你一个下标从 **0** 开始的二维整数数组 `nums` 。

返回位于 `nums` 至少一条 **对角线** 上的最大 **质数** 。如果任一对角线上均不存在质数，返回 *0 。*

注意：

- 如果某个整数大于 `1` ，且不存在除 `1` 和自身之外的正整数因子，则认为该整数是一个质数。
- 如果存在整数 `i` ，使得 `nums[i][i] = val` 或者 `nums[i][nums.length - i - 1]= val` ，则认为整数 `val` 位于 `nums` 的一条对角线上。

![img](https://assets.leetcode.com/uploads/2023/03/06/screenshot-2023-03-06-at-45648-pm.png)

在上图中，一条对角线是 **[1,5,9]** ，而另一条对角线是 **[3,5,7]** 。

 

**示例 1：**

```
输入：nums = [[1,2,3],[5,6,7],[9,10,11]]
输出：11
解释：数字 1、3、6、9 和 11 是所有 "位于至少一条对角线上" 的数字。由于 11 是最大的质数，故返回 11 。
```

**示例 2：**

```
输入：nums = [[1,2,3],[5,17,7],[9,11,10]]
输出：17
解释：数字 1、3、9、10 和 17 是所有满足"位于至少一条对角线上"的数字。由于 17 是最大的质数，故返回 17 。
```

 

**提示：**

- `1 <= nums.length <= 300`
- `nums.length == numsi.length`
- `1 <= nums[i][j] <= 4*106`



### 模拟/质数

定义一个`check`函数来判断质数，随后暴力模拟。==值得注意的是 1 不是质数，比赛时这里挂了好几次==。

```cpp
class Solution {
public:
    bool isPrime(int num)
    {
        int temp=2;
        while(temp*temp<=num)
        {
            if(num%temp==0) return 0;
            temp++;
        }
        return num>=2;
    }
    
    int diagonalPrime(vector<vector<int>>& nums) {
        int ans=0;
        for(int i=0;i<nums.size();i++)
        {
            if(nums[i][i]>ans && isPrime(nums[i][i])) ans=max(ans,nums[i][i]);
            if(nums[i][nums.size()-1-i]>ans && isPrime(nums[i][nums.size()-1-i])) ans=max(ans,nums[i][nums.size()-1-i]);
        }
        return ans;
    }
};
```



---

## Medium 2615. 等值距离和 4 points

给你一个下标从 **0** 开始的整数数组 `nums` 。现有一个长度等于 `nums.length` 的数组 `arr` 。对于满足 `nums[j] == nums[i]` 且 `j != i` 的所有 `j` ，`arr[i]` 等于所有 `|i - j|` 之和。如果不存在这样的 `j` ，则令 `arr[i]` 等于 `0` 。

返回数组 `arr` *。*

 

**示例 1：**

```
输入：nums = [1,3,1,1,2]
输出：[5,0,3,4,0]
解释：
i = 0 ，nums[0] == nums[2] 且 nums[0] == nums[3] 。因此，arr[0] = |0 - 2| + |0 - 3| = 5 。 
i = 1 ，arr[1] = 0 因为不存在值等于 3 的其他下标。
i = 2 ，nums[2] == nums[0] 且 nums[2] == nums[3] 。因此，arr[2] = |2 - 0| + |2 - 3| = 3 。
i = 3 ，nums[3] == nums[0] 且 nums[3] == nums[2] 。因此，arr[3] = |3 - 0| + |3 - 2| = 4 。 
i = 4 ，arr[4] = 0 因为不存在值等于 2 的其他下标。
```

**示例 2：**

```
输入：nums = [0,5,3]
输出：[0,0,0]
解释：因为 nums 中的元素互不相同，对于所有 i ，都有 arr[i] = 0 。
```

 

**提示：**

- `1 <= nums.length <= 105`
- `0 <= nums[i] <= 109`



### 前缀和/哈希表

这题与`周赛338`的`2602.使数组元素全部相等的最少操作次数`的思路有异曲同工之妙，将相同的数进行分组后就变味了2602题，通过面积和前缀和的计算很巧妙地解决问题。

```cpp
class Solution {
public:
    vector<long long> distance(vector<int>& nums) {
        int n=nums.size();
        vector<long long> ans(n,0);
        
        unordered_map<int,vector<long long>> list;
        
        for(int i=0;i<n;i++) list[nums[i]].push_back(i);
        
        for(auto [_,temp]:list)
        {
            vector<long long> sum(temp.size()+1);
            for(int j=1;j<temp.size()+1;j++)
            {
                sum[j]=sum[j-1]+temp[j-1];//计算前缀和
            }
            
            for(int j=0;j<temp.size();j++)
            {   
                
                long long pre=j*temp[j]-sum[j];
                long long after=sum[temp.size()]-sum[j]-temp[j]*(temp.size()-j);
                ans[temp[j]]=pre+after;
            }
            
            
        }
        return ans;
    }
};
```

### 找规律

我在周赛时使用的方法是同组只遍历一次找规律，即观察不同位置`i`元素的结果之间的规律：

```cpp
class Solution {
public:
    vector<long long> distance(vector<int> &nums) {
        int n = nums.size();
        unordered_map<int, vector<int>> groups;
        for (int i = 0; i < n; ++i)
            groups[nums[i]].push_back(i); // 相同元素分到同一组，记录下标

        vector<long long> ans(n);
        for (auto &[_, a]: groups) {
            int m = a.size();
            long long s = 0;
            for (int x: a)
                s += x - a[0]; // a[0] 到其它下标的距离之和
            ans[a[0]] = s;
            for (int i = 1; i < m; ++i)
                // 从计算 a[i-1] 到计算 a[i]，考虑 s 增加了多少
                ans[a[i]] = s += (long long) (i * 2 - m) * (a[i] - a[i - 1]);
        }
        return ans;
    }
};
```



---

## Medium 2616. 最小化数对的最大差值 5 points

给你一个下标从 **0** 开始的整数数组 `nums` 和一个整数 `p` 。请你从 `nums` 中找到 `p` 个下标对，每个下标对对应数值取差值，你需要使得这 `p` 个差值的 **最大值** **最小**。同时，你需要确保每个下标在这 `p` 个下标对中最多出现一次。

对于一个下标对 `i` 和 `j` ，这一对的差值为 `|nums[i] - nums[j]|` ，其中 `|x|` 表示 `x` 的 **绝对值** 。

请你返回 `p` 个下标对对应数值 **最大差值** 的 **最小值** 。

 

**示例 1：**

```
输入：nums = [10,1,2,7,1,3], p = 2
输出：1
解释：第一个下标对选择 1 和 4 ，第二个下标对选择 2 和 5 。
最大差值为 max(|nums[1] - nums[4]|, |nums[2] - nums[5]|) = max(0, 1) = 1 。所以我们返回 1 。
```

**示例 2：**

```
输入：nums = [4,2,1,2], p = 1
输出：0
解释：选择下标 1 和 3 构成下标对。差值为 |2 - 2| = 0 ，这是最大差值的最小值。
```

 

**提示：**

- `1 <= nums.length <= 105`
- `0 <= nums[i] <= 109`
- `0 <= p <= (nums.length)/2`



### 二分

碰到这种最大化最小值的问题使用二分法，并且问什么内容就二分什么内容
		首先判断单调性：如果这个最大差值 max_sub 为a是满足要求的，那么max_sub>a也是满足要求的，max_sub<a则是不满足要求的，故而是可以使用二分法的。
    	那么这个题的主要思路就是，使用二分法来找这个max_sub，另外需要写一个判断函数，判断这个是否能够找到超过p个的差值小于max_sub的元素对。

```cpp
class Solution {
public:
    
    bool check(vector<int> nums,int p,int mid)
    {
        int i=0;
        int cnt=0;
        while(i<nums.size()-1)
        {
            if(abs(nums[i]-nums[i+1])<=mid)
            {
                cnt++;
                i+=2;
            }
            else i++;
        }
        return cnt>=p;
        //如果能找到超过p对的元素对满足要求，则检查通过
        
    }
    
    
    int minimizeMax(vector<int>& nums, int p) {
     	//二分结果
        sort(nums.begin(),nums.end());
        int left=0;
        int n=nums.size();
        int right=nums[n-1]-nums[0];
        while(left<=right)
        {
            int mid=(right-left)/2+left;
            if (check(nums, p, mid))
                right = mid - 1; 
            else left = mid + 1;
        }
        return left;
        
    }
};
```





---

## Hard 2617. 网格图中最少访问的格子数 6 points

给你一个下标从 **0** 开始的 `m x n` 整数矩阵 `grid` 。你一开始的位置在 **左上角** 格子 `(0, 0)` 。

当你在格子 `(i, j)` 的时候，你可以移动到以下格子之一：

- 满足 `j < k <= grid[i][j] + j` 的格子 `(i, k)` （向右移动），或者
- 满足 `i < k <= grid[i][j] + i` 的格子 `(k, j)` （向下移动）。

请你返回到达 **右下角** 格子 `(m - 1, n - 1)` 需要经过的最少移动格子数，如果无法到达右下角格子，请你返回 `-1` 。

 

**示例 1：**

![img](https://assets.leetcode.com/uploads/2023/01/25/ex1.png)

```
输入：grid = [[3,4,2,1],[4,2,3,1],[2,1,0,0],[2,4,0,0]]
输出：4
解释：上图展示了到达右下角格子经过的 4 个格子。
```

**示例 2：**

![img](https://assets.leetcode.com/uploads/2023/01/25/ex2.png)

```
输入：grid = [[3,4,2,1],[4,2,1,1],[2,1,1,0],[3,4,1,0]]
输出：3
解释：上图展示了到达右下角格子经过的 3 个格子。
```

**示例 3：**

![img](https://assets.leetcode.com/uploads/2023/01/26/ex3.png)

```
输入：grid = [[2,1,0],[1,0,0]]
输出：-1
解释：无法到达右下角格子。
```

 

**提示：**

- `m == grid.length`
- `n == grid[i].length`
- `1 <= m, n <= 105`
- `1 <= m * n <= 105`
- `0 <= grid[i][j] < m * n`
- `grid[m - 1][n - 1] == 0`



### DP+单调栈二分优化

```cpp
class Solution {
public:
    int minimumVisitedCells(vector<vector<int>> &grid) {
        int m = grid.size(), n = grid[0].size(), mn;
        vector<vector<pair<int, int>>> col_st(n); // 每列的单调栈
        for (int i = m - 1; i >= 0; --i) {
            vector<pair<int, int>> st; // 当前行的单调栈
            for (int j = n - 1; j >= 0; --j) {
                auto &st2 = col_st[j];
                mn = INT_MAX;
                if (i == m - 1 && j == n - 1) // 特殊情况：已经是终点
                    mn = 0;
                else if (int g = grid[i][j]; g) {
                    // 在单调栈上二分
                    auto it = lower_bound(st.begin(), st.end(), j + g, [](const auto &a, const int b) {
                        return a.second > b;
                    });
                    if (it < st.end()) mn = min(mn, it->first);
                    it = lower_bound(st2.begin(), st2.end(), i + g, [](const auto &a, const int b) {
                        return a.second > b;
                    });
                    if (it < st2.end()) mn = min(mn, it->first);
                }
                if (mn == INT_MAX) continue;

                ++mn; // 加上 (i,j) 这个格子
                // 插入单调栈
                while (!st.empty() && mn <= st.back().first)
                    st.pop_back();
                st.emplace_back(mn, j);
                while (!st2.empty() && mn <= st2.back().first)
                    st2.pop_back();
                st2.emplace_back(mn, i);
            }
        }
        return mn < INT_MAX ? mn : -1;
    }
};

```

> 灵茶山艾府：https://leetcode.cn/problems/minimum-number-of-visited-cells-in-a-grid/solutions/2216329/dan-diao-zhan-you-hua-dp-by-endlesscheng-mc50/