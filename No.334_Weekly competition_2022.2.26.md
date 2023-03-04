<<<<<<< HEAD
# No.334 Weekly competition

## Easy 6369. 左右元素和的差值 3 points

给你一个下标从 **0** 开始的整数数组 `nums` ，请你找出一个下标从 **0** 开始的整数数组 `answer` ，其中：

- `answer.length == nums.length`
- `answer[i] = |leftSum[i] - rightSum[i]|`

其中：

- `leftSum[i]` 是数组 `nums` 中下标 `i` 左侧元素之和。如果不存在对应的元素，`leftSum[i] = 0` 。
- `rightSum[i]` 是数组 `nums` 中下标 `i` 右侧元素之和。如果不存在对应的元素，`rightSum[i] = 0` 。

返回数组 `answer` 。

 

**示例 1：**

```
输入：nums = [10,4,8,3]
输出：[15,1,11,22]
解释：数组 leftSum 为 [0,10,14,22] 且数组 rightSum 为 [15,11,3,0] 。
数组 answer 为 [|0 - 15|,|10 - 11|,|14 - 3|,|22 - 0|] = [15,1,11,22] 。
```

**示例 2：**

```
输入：nums = [1]
输出：[0]
解释：数组 leftSum 为 [0] 且数组 rightSum 为 [0] 。
数组 answer 为 [|0 - 0|] = [0] 。
```

 

**提示：**

- `1 <= nums.length <= 1000`
- `1 <= nums[i] <= 105`

### 简单模拟

```cpp
class Solution {
public:
    vector<int> leftRigthDifference(vector<int>& nums) {
        
        int sum=accumulate(nums.begin(),nums.end(),0);
        int leftnum=0;
        for(int &num : nums)
        {
            int temp=leftnum+num;
            num=abs(sum-temp-leftnum);
            leftnum=temp;
        }
        return nums;
    }
};
```



---

## Medium 6368. 找出字符串的可整除数组 4 points

给你一个下标从 **0** 开始的字符串 `word` ，长度为 `n` ，由从 `0` 到 `9` 的数字组成。另给你一个正整数 `m` 。

`word` 的 **可整除数组** `div` 是一个长度为 `n` 的整数数组，并满足：

- 如果 `word[0,...,i]` 所表示的 **数值** 能被 `m` 整除，`div[i] = 1`
- 否则，`div[i] = 0`

返回 `word` 的可整除数组。

 

**示例 1：**

```
输入：word = "998244353", m = 3
输出：[1,1,0,0,0,1,1,0,0]
解释：仅有 4 个前缀可以被 3 整除："9"、"99"、"998244" 和 "9982443" 。
```

**示例 2：**

```
输入：word = "1010", m = 10
输出：[0,1,0,1]
解释：仅有 2 个前缀可以被 10 整除："10" 和 "1010" 。
```

 

**提示：**

- `1 <= n <= 105`
- `word.length == n`
- `word` 由数字 `0` 到 `9` 组成
- `1 <= m <= 109`



### 错误方法：大数越界

未考虑大数情况，在数字位数很长的这种情况下，`temp=temp*10+ch-'0';`会发生溢出情况

例如测试用例：

`word = "641770318471366266679729826266679777211350732329811266679745308703298458572010861577626667968134562136326667976028761918809582469329982666796698426667978152666797884951266679774262666797857121803678266679750689"`

`m=266679793`

```cpp
class Solution {
public:
    vector<int> divisibilityArray(string word, int m) {
        vector<int> res;
        int temp=0;
        for(char ch:word)
        {
            temp=temp*10+ch-'0';
            if(temp % m == 0) res.push_back(1);
            else res.push_back(0);
        }
        return res;
    }
};
```

### 解决方法：

```cpp
class Solution {
public:
    vector<int> divisibilityArray(string word, int m) {
        vector<int> res;
        long long temp=0;//因为面对 m > 2^31 / 10 这种情况时会出现越界，所以使用了long long
        for(int i=0;i<word.size();i++)
        {
            if(!i) res.push_back((temp=((word[i]-'0') % m))==0);
            else 
            {
                if((temp=((temp*10+word[i]-'0') % m))==0) res.push_back(1);//余数 * 10 + 末尾数 再取余，类似于除法原理
                else res.push_back(0);
            }
        }
        return res;
    }
};
```

我的解决方法：使用类似除法原理（（余数 * 10 + 末尾数）% m），但这仍然会造成大数越界的问题，必须使用 long long temp。



**其实也就是如下的方法原理：**

如果有 $a=k_1m+r_1$ , $b=k_2m+r_2$ ,则有：

$(a+b)\  mod \ m=(r_1+r_2)\ mod \ m=(a\ mod \ m+b\ mod \ m)\ mod \ m$



---

## Medium 6367. 求出最多标记下标 5 points

给你一个下标从 **0** 开始的整数数组 `nums` 。

一开始，所有下标都没有被标记。你可以执行以下操作任意次：

- 选择两个 **互不相同且未标记** 的下标 `i` 和 `j` ，满足 `2 * nums[i] <= nums[j]` ，标记下标 `i` 和 `j` 。

请你执行上述操作任意次，返回 `nums` 中最多可以标记的下标数目。

 

**示例 1：**

```
输入：nums = [3,5,2,4]
输出：2
解释：第一次操作中，选择 i = 2 和 j = 1 ，操作可以执行的原因是 2 * nums[2] <= nums[1] ，标记下标 2 和 1 。
没有其他更多可执行的操作，所以答案为 2 。
```

**示例 2：**

```
输入：nums = [9,2,5,4]
输出：4
解释：第一次操作中，选择 i = 3 和 j = 0 ，操作可以执行的原因是 2 * nums[3] <= nums[0] ，标记下标 3 和 0 。
第二次操作中，选择 i = 1 和 j = 2 ，操作可以执行的原因是 2 * nums[1] <= nums[2] ，标记下标 1 和 2 。
没有其他更多可执行的操作，所以答案为 4 。
```

**示例 3：**

```
输入：nums = [7,6,8]
输出：0
解释：没有任何可以执行的操作，所以答案为 0 。
```

 

**提示：**

- `1 <= nums.length <= 105`
- `1 <= nums[i] <= 109`



解题的终极思路：

> **结论：从小到大排序后，如果存在 k 对匹配，那么一定可以让最小的 k 个数和最大的 k个数匹配。**



### 二分法

```cpp
class Solution {
public:
    int maxNumOfMarkedIndices(vector<int> &nums) {
        sort(nums.begin(), nums.end());

        auto check = [&](int k) -> bool {
            for (int i = 0; i < k; ++i)
                if (nums[i] * 2 > nums[nums.size() - k + i])//判断是否连续的k个数都满足
                    return false;
            return true;
        };

        int left = 0, right = nums.size() / 2 ; // 闭区间
        while (left <= right) {//二分法找到 <= target 的位置 
            int mid = left + (right - left) / 2;
            if(check(mid)) left=mid+1;
            else right=mid-1;
        }
        return (left-1) * 2;
    }
};
```

二分的思路很容易想到，但是这题的关键在于连续的前 k 个数和最后 k 个数对应。

### 双指针

```cpp
class Solution {
public:
    int maxNumOfMarkedIndices(vector<int> &nums) {
        sort(nums.begin(), nums.end());
        int i = 0, n = nums.size();
        //从中点向后一直进行搜索直到最后一个元素，即找到最后的 k 个元素
        for (int j = (n + 1) / 2; j < n; j++)
            if (nums[i] * 2 <= nums[j])//如果满足条件，则 k + 1（i 匹配这个 j），如果不满足条件，则这个 j 不满足对应的 i ，i 则不变 j++ 进行下一次判断，实际上相当于将此前满足条件的初始点向后移（同样是满足条件的） 
                i++;
        return i * 2;
    }
};
```

同样的思路，使用双指针，并用其中的指针 i 来记录个数 k 。



---

## Hard 2577. 在网格图中访问一个格子的最少时间 6 points

给你一个 `m x n` 的矩阵 `grid` ，每个元素都为 **非负** 整数，其中 `grid[row][col]` 表示可以访问格子 `(row, col)` 的 **最早** 时间。也就是说当你访问格子 `(row, col)` 时，最少已经经过的时间为 `grid[row][col]` 。

你从 **最左上角** 出发，出发时刻为 `0` ，你必须一直移动到上下左右相邻四个格子中的 **任意** 一个格子（即不能停留在格子上）。每次移动都需要花费 1 单位时间。

请你返回 **最早** 到达右下角格子的时间，如果你无法到达右下角的格子，请你返回 `-1` 。

 

**示例 1：**

![img](https://assets.leetcode.com/uploads/2023/02/14/yetgriddrawio-8.png)

```
输入：grid = [[0,1,3,2],[5,1,2,5],[4,3,8,6]]
输出：7
解释：一条可行的路径为：
- 时刻 t = 0 ，我们在格子 (0,0) 。
- 时刻 t = 1 ，我们移动到格子 (0,1) ，可以移动的原因是 grid[0][1] <= 1 。
- 时刻 t = 2 ，我们移动到格子 (1,1) ，可以移动的原因是 grid[1][1] <= 2 。
- 时刻 t = 3 ，我们移动到格子 (1,2) ，可以移动的原因是 grid[1][2] <= 3 。
- 时刻 t = 4 ，我们移动到格子 (1,1) ，可以移动的原因是 grid[1][1] <= 4 。
- 时刻 t = 5 ，我们移动到格子 (1,2) ，可以移动的原因是 grid[1][2] <= 5 。
- 时刻 t = 6 ，我们移动到格子 (1,3) ，可以移动的原因是 grid[1][3] <= 6 。
- 时刻 t = 7 ，我们移动到格子 (2,3) ，可以移动的原因是 grid[2][3] <= 7 。
最终到达时刻为 7 。这是最早可以到达的时间。
```

**示例 2：**

![img](https://assets.leetcode.com/uploads/2023/02/14/yetgriddrawio-9.png)

```
输入：grid = [[0,2,4],[3,2,1],[1,0,4]]
输出：-1
解释：没法从左上角按题目规定走到右下角。
```

 

**提示：**

- `m == grid.length`
- `n == grid[i].length`
- `2 <= m, n <= 1000`
- `4 <= m * n <= 105`
- `0 <= grid[i][j] <= 105`
- `grid[0][0] == 0`



### Dijkstra 算法

```cpp
class Solution {
    static constexpr int dirs[4][2] = {{-1, 0}, {1, 0}, {0, -1}, {0, 1}};//方向数组
    //关于 constexpr 可以参考CSDN：https://blog.csdn.net/qq_37766667/article/details/123915233
public:
    int minimumTime(vector<vector<int>> &grid) {
        int m = grid.size(), n = grid[0].size();
        if (grid[0][1] > 1 && grid[1][0] > 1) //只有这种情况下是不可能到到达目的地的，因为第一步都无法迈出
            return -1;
        
        //其他时候都是一定可以的，因为我可以在满足要求的两个节点间反复横跳来拖时间，直到满足时间（绝了，就硬拖）
        
        //我需要知道到达 grid[i][j] 的最小时间 dis[i][j] 
		// dis[i][j] 一定和 (i+j) 是同奇偶的，因为不考虑其他情况下，dis[i][j] == i + j，现在每一次反复横跳 +2 ，同奇偶
		//如果没有同奇偶的话，dis[i][j]+=1就好了，这里没有同奇偶的原因在下面
        /*
        在进行对到达的点的最小路径进行判断的时候，需要进行比较，这时 d+1 一定是满足奇偶性要求的，但是 grid[x][y] 是题目给的可能是不满足的
        int nd = max(d + 1, grid[x][y]);
        */
		//边权与格子的值和位置有关

        

        int dis[m][n];
        memset(dis, 0x3f, sizeof(dis));
        dis[0][0] = 0;
        priority_queue<tuple<int, int, int>, vector<tuple<int, int, int>>, greater<>> pq;
        //定义升序优先队列，小顶堆，内含元素为元组 <dis,i,j>
        pq.emplace(0, 0, 0);//初始状态
        for (;;) {//死循环，因为最终一定可以找到路径
            auto[d, i, j] = pq.top();//弹出当前dis最小的
            pq.pop();
            if (i == m - 1 && j == n - 1)//如果已经到达终点 直接返回 dis
                return d;
            for (auto &q : dirs) { // 枚举周围四个格子
                int x = i + q[0], y = j + q[1];
                if (0 <= x && x < m && 0 <= y && y < n) {//判断是否越界，不越界则继续
                    int nd = max(d + 1, grid[x][y]);//当前的到这个点的时间和最早到这个点的时间的较大值
                    nd += (nd - x - y) % 2; // nd 必须和 x+y 同奇偶
                    if (nd < dis[x][y]) {//当前得到的最短路比记录的最短路小时
                        dis[x][y] = nd; // 更新最短路
                        pq.emplace(nd, x, y);//该点的最短路径记录添加入堆
                    }
                }
            }
        }
    }
};
```

这道题推荐参考 [灵茶山艾府](https://space.bilibili.com/206214) 的讲解，我在这里也进行了逐条的注释，比较厉害的是奇偶性的结论。

> 题解：https://leetcode.cn/problems/minimum-time-to-visit-a-cell-in-a-grid/solutions/2134200/er-fen-da-an-bfspythonjavacgo-by-endless-j10w/









=======
# No.334 Weekly competition

## Easy 6369. 左右元素和的差值 3 points

给你一个下标从 **0** 开始的整数数组 `nums` ，请你找出一个下标从 **0** 开始的整数数组 `answer` ，其中：

- `answer.length == nums.length`
- `answer[i] = |leftSum[i] - rightSum[i]|`

其中：

- `leftSum[i]` 是数组 `nums` 中下标 `i` 左侧元素之和。如果不存在对应的元素，`leftSum[i] = 0` 。
- `rightSum[i]` 是数组 `nums` 中下标 `i` 右侧元素之和。如果不存在对应的元素，`rightSum[i] = 0` 。

返回数组 `answer` 。

 

**示例 1：**

```
输入：nums = [10,4,8,3]
输出：[15,1,11,22]
解释：数组 leftSum 为 [0,10,14,22] 且数组 rightSum 为 [15,11,3,0] 。
数组 answer 为 [|0 - 15|,|10 - 11|,|14 - 3|,|22 - 0|] = [15,1,11,22] 。
```

**示例 2：**

```
输入：nums = [1]
输出：[0]
解释：数组 leftSum 为 [0] 且数组 rightSum 为 [0] 。
数组 answer 为 [|0 - 0|] = [0] 。
```

 

**提示：**

- `1 <= nums.length <= 1000`
- `1 <= nums[i] <= 105`

### 简单模拟

```cpp
class Solution {
public:
    vector<int> leftRigthDifference(vector<int>& nums) {
        
        int sum=accumulate(nums.begin(),nums.end(),0);
        int leftnum=0;
        for(int &num : nums)
        {
            int temp=leftnum+num;
            num=abs(sum-temp-leftnum);
            leftnum=temp;
        }
        return nums;
    }
};
```



---

## Medium 6368. 找出字符串的可整除数组 4 points

给你一个下标从 **0** 开始的字符串 `word` ，长度为 `n` ，由从 `0` 到 `9` 的数字组成。另给你一个正整数 `m` 。

`word` 的 **可整除数组** `div` 是一个长度为 `n` 的整数数组，并满足：

- 如果 `word[0,...,i]` 所表示的 **数值** 能被 `m` 整除，`div[i] = 1`
- 否则，`div[i] = 0`

返回 `word` 的可整除数组。

 

**示例 1：**

```
输入：word = "998244353", m = 3
输出：[1,1,0,0,0,1,1,0,0]
解释：仅有 4 个前缀可以被 3 整除："9"、"99"、"998244" 和 "9982443" 。
```

**示例 2：**

```
输入：word = "1010", m = 10
输出：[0,1,0,1]
解释：仅有 2 个前缀可以被 10 整除："10" 和 "1010" 。
```

 

**提示：**

- `1 <= n <= 105`
- `word.length == n`
- `word` 由数字 `0` 到 `9` 组成
- `1 <= m <= 109`



### 错误方法：大数越界

未考虑大数情况，在这种情况下，`temp=temp*10+ch-'0';`会发生溢出情况

```cpp
class Solution {
public:
    vector<int> divisibilityArray(string word, int m) {
        vector<int> res;
        int temp=0;
        for(char ch:word)
        {
            temp=temp*10+ch-'0';
            if(temp % m == 0) res.push_back(1);
            else res.push_back(0);
        }
        return res;
    }
};
```

### 解决方法：

```cpp
class Solution {
public:
    vector<int> divisibilityArray(string word, int m) {
        vector<int> res;
        long long temp=0;
        for(int i=0;i<word.size();i++)
        {
            if(!i) res.push_back((temp=((word[i]-'0') % m))==0);
            else 
            {
                if((temp=((temp*10+word[i]-'0') % m))==0) res.push_back(1);
                else res.push_back(0);
            }
        }
        return res;
    }
};
```



---

## Medium 6367. 求出最多标记下标 5 points

给你一个下标从 **0** 开始的整数数组 `nums` 。

一开始，所有下标都没有被标记。你可以执行以下操作任意次：

- 选择两个 **互不相同且未标记** 的下标 `i` 和 `j` ，满足 `2 * nums[i] <= nums[j]` ，标记下标 `i` 和 `j` 。

请你执行上述操作任意次，返回 `nums` 中最多可以标记的下标数目。

 

**示例 1：**

```
输入：nums = [3,5,2,4]
输出：2
解释：第一次操作中，选择 i = 2 和 j = 1 ，操作可以执行的原因是 2 * nums[2] <= nums[1] ，标记下标 2 和 1 。
没有其他更多可执行的操作，所以答案为 2 。
```

**示例 2：**

```
输入：nums = [9,2,5,4]
输出：4
解释：第一次操作中，选择 i = 3 和 j = 0 ，操作可以执行的原因是 2 * nums[3] <= nums[0] ，标记下标 3 和 0 。
第二次操作中，选择 i = 1 和 j = 2 ，操作可以执行的原因是 2 * nums[1] <= nums[2] ，标记下标 1 和 2 。
没有其他更多可执行的操作，所以答案为 4 。
```

**示例 3：**

```
输入：nums = [7,6,8]
输出：0
解释：没有任何可以执行的操作，所以答案为 0 。
```

 

**提示：**

- `1 <= nums.length <= 105`
- `1 <= nums[i] <= 109`



解题的终极思路：

> **结论：从小到大排序后，如果存在 k 对匹配，那么一定可以让最小的 k 个数和最大的 k个数匹配。**



### 二分法

```cpp
class Solution {
public:
    int maxNumOfMarkedIndices(vector<int> &nums) {
        sort(nums.begin(), nums.end());

        auto check = [&](int k) -> bool {
            for (int i = 0; i < k; ++i)
                if (nums[i] * 2 > nums[nums.size() - k + i])//判断是否连续的k个数都满足
                    return false;
            return true;
        };

        int left = 0, right = nums.size() / 2 ; // 闭区间
        while (left <= right) {//二分法找到 <= target 的位置 
            int mid = left + (right - left) / 2;
            if(check(mid)) left=mid+1;
            else right=mid-1;
        }
        return (left-1) * 2;
    }
};
```

二分的思路很容易想到，但是这题的关键在于连续的前 k 个数和最后 k 个数对应。

### 双指针

```cpp
class Solution {
public:
    int maxNumOfMarkedIndices(vector<int> &nums) {
        sort(nums.begin(), nums.end());
        int i = 0, n = nums.size();
        //从中点向后一直进行搜索直到最后一个元素，即找到最后的 k 个元素
        for (int j = (n + 1) / 2; j < n; j++)
            if (nums[i] * 2 <= nums[j])//如果满足条件，则 k + 1（i 匹配这个 j），如果不满足条件，则这个 j 不满足对应的 i ，i 则不变 j++ 进行下一次判断，实际上相当于将此前满足条件的初始点向后移（同样是满足条件的） 
                i++;
        return i * 2;
    }
};
```

同样的思路，使用双指针，并用其中的指针 i 来记录个数 k 。



---

## Hard 2577. 在网格图中访问一个格子的最少时间 6 points

给你一个 `m x n` 的矩阵 `grid` ，每个元素都为 **非负** 整数，其中 `grid[row][col]` 表示可以访问格子 `(row, col)` 的 **最早** 时间。也就是说当你访问格子 `(row, col)` 时，最少已经经过的时间为 `grid[row][col]` 。

你从 **最左上角** 出发，出发时刻为 `0` ，你必须一直移动到上下左右相邻四个格子中的 **任意** 一个格子（即不能停留在格子上）。每次移动都需要花费 1 单位时间。

请你返回 **最早** 到达右下角格子的时间，如果你无法到达右下角的格子，请你返回 `-1` 。

 

**示例 1：**

![img](https://assets.leetcode.com/uploads/2023/02/14/yetgriddrawio-8.png)

```
输入：grid = [[0,1,3,2],[5,1,2,5],[4,3,8,6]]
输出：7
解释：一条可行的路径为：
- 时刻 t = 0 ，我们在格子 (0,0) 。
- 时刻 t = 1 ，我们移动到格子 (0,1) ，可以移动的原因是 grid[0][1] <= 1 。
- 时刻 t = 2 ，我们移动到格子 (1,1) ，可以移动的原因是 grid[1][1] <= 2 。
- 时刻 t = 3 ，我们移动到格子 (1,2) ，可以移动的原因是 grid[1][2] <= 3 。
- 时刻 t = 4 ，我们移动到格子 (1,1) ，可以移动的原因是 grid[1][1] <= 4 。
- 时刻 t = 5 ，我们移动到格子 (1,2) ，可以移动的原因是 grid[1][2] <= 5 。
- 时刻 t = 6 ，我们移动到格子 (1,3) ，可以移动的原因是 grid[1][3] <= 6 。
- 时刻 t = 7 ，我们移动到格子 (2,3) ，可以移动的原因是 grid[2][3] <= 7 。
最终到达时刻为 7 。这是最早可以到达的时间。
```

**示例 2：**

![img](https://assets.leetcode.com/uploads/2023/02/14/yetgriddrawio-9.png)

```
输入：grid = [[0,2,4],[3,2,1],[1,0,4]]
输出：-1
解释：没法从左上角按题目规定走到右下角。
```

 

**提示：**

- `m == grid.length`
- `n == grid[i].length`
- `2 <= m, n <= 1000`
- `4 <= m * n <= 105`
- `0 <= grid[i][j] <= 105`
- `grid[0][0] == 0`



### Dijkstra 算法

```cpp
class Solution {
    static constexpr int dirs[4][2] = {{-1, 0}, {1, 0}, {0, -1}, {0, 1}};//方向数组
    //关于 constexpr 可以参考CSDN：https://blog.csdn.net/qq_37766667/article/details/123915233
public:
    int minimumTime(vector<vector<int>> &grid) {
        int m = grid.size(), n = grid[0].size();
        if (grid[0][1] > 1 && grid[1][0] > 1) //只有这种情况下是不可能到到达目的地的，因为第一步都无法迈出
            return -1;
        
        //其他时候都是一定可以的，因为我可以在满足要求的两个节点间反复横跳来拖时间，直到满足时间（绝了，就硬拖）
        
        //我需要知道到达 grid[i][j] 的最小时间 dis[i][j] 
		// dis[i][j] 一定和 (i+j) 是同奇偶的，因为不考虑其他情况下，dis[i][j] == i + j，现在每一次反复横跳 +2 ，同奇偶
		//如果没有同奇偶的话，dis[i][j]+=1就好了，这里没有同奇偶的原因在下面
        /*
        在进行对到达的点的最小路径进行判断的时候，需要进行比较，这时 d+1 一定是满足奇偶性要求的，但是 grid[x][y] 是题目给的可能是不满足的
        int nd = max(d + 1, grid[x][y]);
        */
		//边权与格子的值和位置有关

        

        int dis[m][n];
        memset(dis, 0x3f, sizeof(dis));
        dis[0][0] = 0;
        priority_queue<tuple<int, int, int>, vector<tuple<int, int, int>>, greater<>> pq;
        //定义升序优先队列，小顶堆，内含元素为元组 <dis,i,j>
        pq.emplace(0, 0, 0);//初始状态
        for (;;) {//死循环，因为最终一定可以找到路径
            auto[d, i, j] = pq.top();//弹出当前dis最小的
            pq.pop();
            if (i == m - 1 && j == n - 1)//如果已经到达终点 直接返回 dis
                return d;
            for (auto &q : dirs) { // 枚举周围四个格子
                int x = i + q[0], y = j + q[1];
                if (0 <= x && x < m && 0 <= y && y < n) {//判断是否越界，不越界则继续
                    int nd = max(d + 1, grid[x][y]);//当前的到这个点的时间和最早到这个点的时间的较大值
                    nd += (nd - x - y) % 2; // nd 必须和 x+y 同奇偶
                    if (nd < dis[x][y]) {//当前得到的最短路比记录的最短路小时
                        dis[x][y] = nd; // 更新最短路
                        pq.emplace(nd, x, y);//该点的最短路径记录添加入堆
                    }
                }
            }
        }
    }
};
```

这道题推荐参考 [灵茶山艾府](https://space.bilibili.com/206214) 的讲解，我在这里也进行了逐条的注释，比较厉害的是奇偶性的结论。

> 题解：https://leetcode.cn/problems/minimum-time-to-visit-a-cell-in-a-grid/solutions/2134200/er-fen-da-an-bfspythonjavacgo-by-endless-j10w/









>>>>>>> 7d85840c26e1c39482acd48895d7992fa0f24274
