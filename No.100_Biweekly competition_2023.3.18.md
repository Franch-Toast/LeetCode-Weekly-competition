

# No.100 Biweekly competition

## Easy 2591. 将钱分给最多的儿童 3 points

- 给你一个整数 `money` ，表示你总共有的钱数（单位为美元）和另一个整数 `children` ，表示你要将钱分配给多少个儿童。

  你需要按照如下规则分配：

  - 所有的钱都必须被分配。
  - 每个儿童至少获得 `1` 美元。
  - 没有人获得 `4` 美元。

  请你按照上述规则分配金钱，并返回 **最多** 有多少个儿童获得 **恰好** `8` 美元。如果没有任何分配方案，返回 `-1` 。

   

  **示例 1：**

  ```
  输入：money = 20, children = 3
  输出：1
  解释：
  最多获得 8 美元的儿童数为 1 。一种分配方案为：
  - 给第一个儿童分配 8 美元。
  - 给第二个儿童分配 9 美元。
  - 给第三个儿童分配 3 美元。
  没有分配方案能让获得 8 美元的儿童数超过 1 。
  ```

  **示例 2：**

  ```
  输入：money = 16, children = 2
  输出：2
  解释：每个儿童都可以获得 8 美元。
  ```

   

  **提示：**

  - `1 <= money <= 200`
  - `2 <= children <= 30`



### 逻辑/公式

Wrong Answer 了3次才把逻辑捋通顺，得到以下代码：

```cpp
class Solution {
public:
    int distMoney(int money, int children) {
        //题目可以转换为：先给每个小孩分1块钱，然后再尽量分给尽可能多的小孩7块，同时不能有小孩分到3块
        if(money<children) return -1;
        money-=children;
        int ans=money/7;//首先看能分成几个7块
        if(ans>children) return children-1;//当分配次数一定超过children数时，一定返回children-1
        else if(ans==children && money%7==0) return children;//当分配次数==children数且钱分完了，返回children
        else if(ans==children && money%7!=0) return children-1;//当分配次数==children数且钱没分完，委屈一个children收下剩余的钱
        else
        {
            if(ans==children-1)//当分配次数==children-1时
            {
                if(money%7!=3) return ans;//如果没剩三块钱，则直接分配即可
                else return ans-1;//如果恰好只剩三块钱，那么也要委屈一个孩子和最后一个孩子合作分摊一下
            }
            else return ans;//当分配次数<children-1，不管剩多少钱都可以让剩下的孩子不会收到三块钱
        }
    }
};
```

灵神给出的做法也同样是逻辑 + 分类讨论：

```cpp
class Solution {
public:
    int distMoney(int money, int children) {
        money -= children; // 每人至少 1 美元
        if (money < 0) return -1;
        int ans = min(money / 7, children); // 初步分配，让尽量多的人分到 8 美元
        money -= ans * 7;
        children -= ans;
        if (children == 0 && money || // 必须找一个前面分了 8 美元的人，分完剩余的钱
            children == 1 && money == 3) // 不能有人恰好分到 4 美元
            --ans;
        return ans;
    }
};
```



---

## Medium 2592. 最大化数组的伟大值 4 points

给你一个下标从 0 开始的整数数组 `nums` 。你需要将 `nums` 重新排列成一个新的数组 `perm` 。

定义 `nums` 的 **伟大值** 为满足 `0 <= i < nums.length` 且 `perm[i] > nums[i]` 的下标数目。

请你返回重新排列 `nums` 后的 **最大** 伟大值。

 

**示例 1：**

```
输入：nums = [1,3,5,2,1,3,1]
输出：4
解释：一个最优安排方案为 perm = [2,5,1,3,3,1,1] 。
在下标为 0, 1, 3 和 4 处，都有 perm[i] > nums[i] 。因此我们返回 4 。
```

**示例 2：**

```
输入：nums = [1,2,3,4]
输出：3
解释：最优排列为 [2,3,4,1] 。
在下标为 0, 1 和 2 处，都有 perm[i] > nums[i] 。因此我们返回 3 。
```

 

**提示：**

- `1 <= nums.length <= 105`
- `0 <= nums[i] <= 109`



### 排序/双指针遍历

我的思路其实很简单，由于只要最后给出为最大的伟大值，那么可以进行排序，然后使用双指针，让数组中的`当前次小数>当前最小数`，这样就可以贡献一个伟大值，一直向后查找，直到最大值已经作为次小数被匹配了，则结束。至于最后面的数则可以放弃，用最小的数将就一下。

```cpp
class Solution {
public:
    int maximizeGreatness(vector<int>& nums) {
        int ans=0;
        sort(nums.begin(),nums.end());
        for(int num:nums)//作为次大数进行匹配
        {
            if(num>nums[ans]) ans++;//如果该数是比当前nums[ans]大的次大数，则匹配下一个最小数
            //如果不是当前次小数，例如和最小数是一样大的，则最小数指针不前进，反而是大数前进
        }
        return ans;
    }
};
```

### 双指针距离

很奇妙的一个做法，时间复杂度和空间复杂度都是 O(n)。由于存在相同的数时，双指针要满足要求一定会间隔相同的数的数量的距离，找到这个相同的数的最大的距离即为双指针间隔的最大距离，此时双指针之间距离在到达终点时一定会保持这个距离，所以只要统计数组中最大的相同的数的数量 mx ，返回 nums.size() - mx 即可。

```cpp
class Solution {
public:
    int maximizeGreatness(vector<int> &nums) {
        int mx = 0;
        unordered_map<int, int> cnt;
        for (int x : nums)
            mx = max(mx, ++cnt[x]);
        return nums.size() - mx;
    }
};
```

> 灵茶山艾府：https://leetcode.cn/problems/maximize-greatness-of-an-array/solutions/2177186/tian-ji-sai-ma-by-endlesscheng-buk1/



---

## Medium 2593. 标记所有元素后数组的分数 4 points

给你一个数组 `nums` ，它包含若干正整数。

一开始分数 `score = 0` ，请你按照下面算法求出最后分数：

- 从数组中选择最小且没有被标记的整数。如果有相等元素，选择下标最小的一个。
- 将选中的整数加到 `score` 中。
- 标记 **被选中元素**，如果有相邻元素，则同时标记 **与它相邻的两个元素** 。
- 重复此过程直到数组中所有元素都被标记。

请你返回执行上述算法后最后的分数。

 

**示例 1：**

```
输入：nums = [2,1,3,4,5,2]
输出：7
解释：我们按照如下步骤标记元素：
- 1 是最小未标记元素，所以标记它和相邻两个元素：[2,1,3,4,5,2] 。
- 2 是最小未标记元素，所以标记它和左边相邻元素：[2,1,3,4,5,2] 。
- 4 是仅剩唯一未标记的元素，所以我们标记它：[2,1,3,4,5,2] 。
总得分为 1 + 2 + 4 = 7 。
```

**示例 2：**

```
输入：nums = [2,3,5,1,3,2]
输出：5
解释：我们按照如下步骤标记元素：
- 1 是最小未标记元素，所以标记它和相邻两个元素：[2,3,5,1,3,2] 。
- 2 是最小未标记元素，由于有两个 2 ，我们选择最左边的一个 2 ，也就是下标为 0 处的 2 ，以及它右边相邻的元素：[2,3,5,1,3,2] 。
- 2 是仅剩唯一未标记的元素，所以我们标记它：[2,3,5,1,3,2] 。
总得分为 1 + 2 + 2 = 5 。
```

 

**提示：**

- `1 <= nums.length <= 105`
- `1 <= nums[i] <= 106`



### 稳定排序

把 nums[i]及其下标绑定后，按照元素值从小到大排序，元素值相同的按照下标排序。

然后按照题目模拟，用一个 vis 数组来实现标记。也可以生成一个下标数组，对下标排序。

```cpp
class Solution {
public:
    long long findScore(vector<int> &nums) {
        int n = nums.size(), ids[n];
        iota(ids, ids + n, 0);//ids数组从首地址开始从 0 递增地填充到 n-1
        stable_sort(ids, ids + n, [&](int i, int j) {//stable_sort() 函数在功能上除了可以实现排序，还可以保证不改变相等元素的相对位置，语法格式也是相同的
            return nums[i] < nums[j];//按 nums 从小到大排序索引
        });

        long long ans = 0;
        bool vis[n + 2];  // 保证下标不越界
        memset(vis, 0, sizeof(vis));//填充为 0 ，表示都没有访问过
        for (int i : ids)//从最小的元素索引开始
            if (!vis[i + 1]) {//这里用了一个小技巧我一开始怎么也想不通，直接把所有地索引后移一位，这样虽然看上去不对应，但是在物理上是对应的，因为我的空间是够的，所以每次的 i 我只要查询 i+1 有没有被访问过即可！
                vis[i] = vis[i + 2] = true;
                ans += nums[i];
            }
        return ans;
    }
};
```



---

## Medium 2594. 修车的最少时间 5 points

- 给你一个整数数组 `ranks` ，表示一些机械工的 **能力值** 。`ranksi` 是第 `i` 位机械工的能力值。能力值为 `r` 的机械工可以在 `r * n2` 分钟内修好 `n` 辆车。

  同时给你一个整数 `cars` ，表示总共需要修理的汽车数目。

  请你返回修理所有汽车 **最少** 需要多少时间。

  **注意：**所有机械工可以同时修理汽车。

   

  **示例 1：**

  ```
  输入：ranks = [4,2,3,1], cars = 10
  输出：16
  解释：
  - 第一位机械工修 2 辆车，需要 4 * 2 * 2 = 16 分钟。
  - 第二位机械工修 2 辆车，需要 2 * 2 * 2 = 8 分钟。
  - 第三位机械工修 2 辆车，需要 3 * 2 * 2 = 12 分钟。
  - 第四位机械工修 4 辆车，需要 1 * 4 * 4 = 16 分钟。
  16 分钟是修理完所有车需要的最少时间。
  ```

  **示例 2：**

  ```
  输入：ranks = [5,1,8], cars = 6
  输出：16
  解释：
  - 第一位机械工修 1 辆车，需要 5 * 1 * 1 = 5 分钟。
  - 第二位机械工修 4 辆车，需要 1 * 4 * 4 = 16 分钟。
  - 第三位机械工修 1 辆车，需要 8 * 1 * 1 = 8 分钟。
  16 分钟时修理完所有车需要的最少时间。
  ```

   

  **提示：**

  - `1 <= ranks.length <= 105`
  - `1 <= ranks[i] <= 100`
  - `1 <= cars <= 106`



### 二分

`r*n*n`是单调函数，故而可以使用二分法。
如果能用 t 时间完成，那么t+1、t+2 时间也可以完成；反之，如果无法用 t 时间完成，那么t-1、t-2 时间也不能完成

二分法的思路：假设时间 t 是我要求的时间
则有：每一个机械工修车的时间需要满足`r*n*n<=t`，得到`n <= floor(sqrt(t // r))`。这里数字的大小不会导致精度问题。
再将所有机械工在 t 时间内能够修的车的数量求和，如果满足车辆要求`sum(n) >= cars `则说明可以用 t 时间完成。

即：以时间 t 为二分目标，以 `sum(n) >= cars `为判断目标进行二分。

```cpp
class Solution {
public:
    long long repairCars(vector<int> &ranks, int cars) {
        int min_r = *min_element(ranks.begin(), ranks.end());//得到ranks中最小能力值的机械工，
        long long left = 0, right = 1LL * min_r * cars * cars;//尽量缩小范围，可以把所有的车都交给能力值最小的机械工，他修得最快。LL其实代表long long,* 1LL是为了在计算时，把int类型的变量转化为long long，然后再赋值给long long类型的变量。
        while (left + 1 < right) { // 开区间
            long long mid = (left + right) / 2, s = 0;
            for (int r : ranks)//以当前的 mid 作为时间对每个机械工在该时间内修的车进行求和判断
                s += sqrt(mid / r);
            (s >= cars ? right : left) = mid;
        }
        return right;
    }
};
```

这里还可以进行优化：将二分的范围进行缩小。由于`ranks[i] <=100`，但是`1 <= ranks.length <= 10^5`，这意味着 ranks 数组中存在着大量重复的能力值，那么可以统计一下每种能力值出现的次数，这样可以大幅度缩小搜索范围。

```cpp
class Solution {
public:
    long long repairCars(vector<int> &ranks, int cars) {
        int min_r = ranks[0], cnt[101]{};
        for (int r : ranks) {
            min_r = min(min_r, r);//找到最小能力值的
            ++cnt[r];//统计同一种能力值的职工的数量
        }
        long long left = 0, right = 1LL * min_r * cars * cars;
        while (left + 1 < right) { // 开区间
            long long mid = (left + right) / 2, s = 0;
            for (int r = min_r; r <= 100; ++r)//以当前的 mid 作为时间对每种能力值的机械工在该时间内修的车进行求和判断
                s += (long long) sqrt(mid / r) * cnt[r];
            (s >= cars ? right : left) = mid;
        }
        return right;
    }
};
```



