# No.337 Weekly competition

## Easy 2595. 奇偶位数 3 points

- 给你一个 **正** 整数 `n` 。

  用 `even` 表示在 `n` 的二进制形式（下标从 **0** 开始）中值为 `1` 的偶数下标的个数。

  用 `odd` 表示在 `n` 的二进制形式（下标从 **0** 开始）中值为 `1` 的奇数下标的个数。

  返回整数数组 `answer` ，其中 `answer = [even, odd]` 。

   

  **示例 1：**

  ```
  输入：n = 17
  输出：[2,0]
  解释：17 的二进制形式是 10001 。 
  下标 0 和 下标 4 对应的值为 1 。 
  共有 2 个偶数下标，0 个奇数下标。
  ```

  **示例 2：**

  ```
  输入：n = 2
  输出：[0,1]
  解释：2 的二进制形式是 10 。 
  下标 1 对应的值为 1 。 
  共有 0 个偶数下标，1 个奇数下标。
  ```

   

  **提示：**

  - `1 <= n <= 1000`

### 模拟

```cpp
class Solution {
public:
    vector<int> evenOddBit(int n) {
        vector<int> ans(2);
        int even=0;
        int odd=0;
        int i=0;
        while(n!=0)
        {
            if(i%2==1) even+=(n & 1);//奇数位，最后一位进行 & 1 运算，其他位 & 0 运算，结果只会与最后一位有关
            else odd+=(n & 1);//偶数位
            n= n>>1;
            i++;
        }
        ans[0]=odd;
        ans[1]=even;
        return ans;
    }
};
```

灵神给出了更简洁的代码：

```cpp
class Solution {
public:
    vector<int> evenOddBit(int n) {
        vector<int> ans(2);
        for (int i = 0; n; i ^= 1, n >>= 1)// i ^= 1,由于 i 初始为 0 ，故而 i 会在0和1间不断转换
            ans[i] += n & 1;
        return ans;
    }
};
```



---

## Medium 2596. 检查骑士巡视方案 4 points

- 骑士在一张 `n x n` 的棋盘上巡视。在有效的巡视方案中，骑士会从棋盘的 **左上角** 出发，并且访问棋盘上的每个格子 **恰好一次** 。

  给你一个 `n x n` 的整数矩阵 `grid` ，由范围 `[0, n * n - 1]` 内的不同整数组成，其中 `grid[row][col]` 表示单元格 `(row, col)` 是骑士访问的第 `grid[row][col]` 个单元格。骑士的行动是从下标 **0** 开始的。

  如果 `grid` 表示了骑士的有效巡视方案，返回 `true`；否则返回 `false`。

  **注意**，骑士行动时可以垂直移动两个格子且水平移动一个格子，或水平移动两个格子且垂直移动一个格子。下图展示了骑士从某个格子出发可能的八种行动路线。
  ![img](https://assets.leetcode.com/uploads/2018/10/12/knight.png)

   

  **示例 1：**

  ![img](https://assets.leetcode.com/uploads/2022/12/28/yetgriddrawio-5.png)

  ```
  输入：grid = [[0,11,16,5,20],[17,4,19,10,15],[12,1,8,21,6],[3,18,23,14,9],[24,13,2,7,22]]
  输出：true
  解释：grid 如上图所示，可以证明这是一个有效的巡视方案。
  ```

  **示例 2：**

  ![img](https://assets.leetcode.com/uploads/2022/12/28/yetgriddrawio-6.png)

  ```
  输入：grid = [[0,3,6],[5,8,1],[2,7,4]]
  输出：false
  解释：grid 如上图所示，考虑到骑士第 7 次行动后的位置，第 8 次行动是无效的。
  ```

   

  **提示：**

  - `n == grid.length == grid[i].length`
  - `3 <= n <= 7`
  - `0 <= grid[row][col] < n * n`
  - `grid` 中的所有整数 **互不相同**



### 模拟

将所有的格子的信息都遍历一遍按访问顺序存储起来，再按照该访问顺序进行判断是否满足马走日的要求。

```cpp
class Solution {
public:
    bool checkValidGrid(vector<vector<int>>& grid) {
        int n=grid.size();
        vector<pair<int,int>> cur(n*n,{0,0});
        for(int i=0;i<n;i++)
        {
            for(int j=0;j<n;j++)
            {
                cur[grid[i][j]]={i,j};//按访问顺序进行存放
            }
        }
        if(cur[0].first!=0 || cur[0].second!=0) return false;//判断是否从左上角出发
        for(int i=0;i<n*n-1;i++)
        {
            int dx=abs(cur[i+1].first-cur[i].first);
            int dy=abs(cur[i+1].second-cur[i].second);
            if((dx*dx+dy*dy)!=5) return false;// (2,1)和(1,2)的结果都会是两者平方和为 5 
        }
        return true;
    }
};
```



---

## Medium 2597. 美丽子集的数目 5 points

给你一个由正整数组成的数组 `nums` 和一个 **正** 整数 `k` 。

如果 `nums` 的子集中，任意两个整数的绝对差均不等于 `k` ，则认为该子数组是一个 **美丽** 子集。

返回数组 `nums` 中 **非空** 且 **美丽** 的子集数目。

`nums` 的子集定义为：可以经由 `nums` 删除某些元素（也可能不删除）得到的一个数组。只有在删除元素时选择的索引不同的情况下，两个子集才会被视作是不同的子集。

 

**示例 1：**

```
输入：nums = [2,4,6], k = 2
输出：4
解释：数组 nums 中的美丽子集有：[2], [4], [6], [2, 6] 。
可以证明数组 [2,4,6] 中只存在 4 个美丽子集。
```

**示例 2：**

```
输入：nums = [1], k = 1
输出：1
解释：数组 nums 中的美丽数组有：[1] 。
可以证明数组 [1] 中只存在 1 个美丽子集。 
```

 

**提示：**

- `1 <= nums.length <= 20`
- `1 <= nums[i], k <= 1000`



### 回溯

和DP的感觉很像，注意的是在递归的过程中需要恢复现场！也可以使用哈希表来做，但是数组更快。

```cpp
class Solution {
public:
    int beautifulSubsets(vector<int> &nums, int k) {
        int ans = -1; // 去掉空集
        int cnt[3001]{}; // 用数组实现比哈希表更快
        function<void(int)> dfs = [&](int i) {
            if (i == nums.size()) {
                ans++;
                return;
            }
            dfs(i + 1); // 不选
            int x = nums[i] + k; // 避免负数下标
            if (cnt[x - k] == 0 && cnt[x + k] == 0) {
                ++cnt[x]; // 选
                dfs(i + 1);
                --cnt[x]; // 恢复现场
            }
        };
        dfs(0);
        return ans;
    }
};
```



---

## Medium 2598. 执行操作后的最大 MEX 5 points

给你一个下标从 **0** 开始的整数数组 `nums` 和一个整数 `value` 。

在一步操作中，你可以对 `nums` 中的任一元素加上或减去 `value` 。

- 例如，如果 `nums = [1,2,3]` 且 `value = 2` ，你可以选择 `nums[0]` 减去 `value` ，得到 `nums = [-1,2,3]` 。

数组的 MEX (minimum excluded) 是指其中数组中缺失的最小非负整数。

- 例如，`[-1,2,3]` 的 MEX 是 `0` ，而 `[1,0,3]` 的 MEX 是 `2` 。

返回在执行上述操作 **任意次** 后，`nums` 的最大 MEX *。*

 

**示例 1：**

```
输入：nums = [1,-10,7,13,6,8], value = 5
输出：4
解释：执行下述操作可以得到这一结果：
- nums[1] 加上 value 两次，nums = [1,0,7,13,6,8]
- nums[2] 减去 value 一次，nums = [1,0,2,13,6,8]
- nums[3] 减去 value 两次，nums = [1,0,2,3,6,8]
nums 的 MEX 是 4 。可以证明 4 是可以取到的最大 MEX 。
```

**示例 2：**

```
输入：nums = [1,-10,7,13,6,8], value = 7
输出：2
解释：执行下述操作可以得到这一结果：
- nums[2] 减去 value 一次，nums = [1,-10,0,13,6,8]
nums 的 MEX 是 2 。可以证明 2 是可以取到的最大 MEX 。
```

 

**提示：**

- `1 <= nums.length, value <= 105`
- `-109 <= nums[i] <= 109`



### 哈希表

判断是否有数字能够通过加减 value 变为 0 ，随后再判断是否有数字能够通过加减 value 变为 1 ，随后再逐一判断过去。

判断能否成功的方法：取模。值得注意的是负数取模会是负数，所以需要进行以下操作：$((m \% value) + value) \%value$

再通过哈希表的方法统计。

```cpp
class Solution {
public:
    int findSmallestInteger(vector<int>& nums, int value) {
        unordered_map<int,int> list;
        for(int num:nums)
        {
            list[((num % value) + value) %value]++;//注意这里的取余方法
        }
        int mex=0;
        while(list[mex % value]) 
        //注意这里需要为mex % value，因为当mex >= value时，哈希表中不会存在key==mex的值了，而会继续循环 
        {
            list[mex % value]--;
            mex++;
        }
        return mex;
    }
};
```

