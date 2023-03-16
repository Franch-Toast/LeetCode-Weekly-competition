# No.336 Weekly competition

## Easy 2586. 统计范围内的元音字符串数 3 points

给你一个下标从 **0** 开始的字符串数组 `words` 和两个整数：`left` 和 `right` 。

如果字符串以元音字母开头并以元音字母结尾，那么该字符串就是一个 **元音字符串** ，其中元音字母是 `'a'`、`'e'`、`'i'`、`'o'`、`'u'` 。

返回 `words[i]` 是元音字符串的数目，其中 `i` 在闭区间 `[left, right]` 内。

 

**示例 1：**

```
输入：words = ["are","amy","u"], left = 0, right = 2
输出：2
解释：
- "are" 是一个元音字符串，因为它以 'a' 开头并以 'e' 结尾。
- "amy" 不是元音字符串，因为它没有以元音字母结尾。
- "u" 是一个元音字符串，因为它以 'u' 开头并以 'u' 结尾。
在上述范围中的元音字符串数目为 2 。
```

**示例 2：**

```
输入：words = ["hey","aeo","mu","ooo","artro"], left = 1, right = 4
输出：3
解释：
- "aeo" 是一个元音字符串，因为它以 'a' 开头并以 'o' 结尾。
- "mu" 不是元音字符串，因为它没有以元音字母开头。
- "ooo" 是一个元音字符串，因为它以 'o' 开头并以 'o' 结尾。
- "artro" 是一个元音字符串，因为它以 'a' 开头并以 'o' 结尾。
在上述范围中的元音字符串数目为 3 。
```

 

**提示：**

- `1 <= words.length <= 1000`
- `1 <= words[i].length <= 10`
- `words[i]` 仅由小写英文字母组成
- `0 <= left <= right < words.length`

### 模拟

```cpp
class Solution {
public:
    int vowelStrings(vector<string>& words, int left, int right) {
        auto test = [&](char c) {
            return c == 'a' || c == 'e' || c == 'i' || c == 'o' || c == 'u';
        };
        int ans = 0;
        for (int i = left; i <= right; i++) if (test(words[i][0]) && test(words[i][words[i].size() - 1])) ans++;
        return ans;
    }
};

```

### 哈希表/模拟

```cpp
class Solution {
public:
    int vowelStrings(vector<string>& words, int left, int right) {
        unordered_set<char> target;
        target.insert('a');
        target.insert('e');
        target.insert('i');
        target.insert('o');
        target.insert('u');

        int ans = 0;
        for (int i = left; i <= right; i++) if (target.count(words[i][0]) && target.count(words[i][words[i].size() - 1])) ans++;
        return ans;
    }
};
```



---

## Medium 2587. 重排数组以得到最大前缀分数 4 points

给你一个下标从 **0** 开始的整数数组 `nums` 。你可以将 `nums` 中的元素按 **任意顺序** 重排（包括给定顺序）。

令 `prefix` 为一个数组，它包含了 `nums` 重新排列后的前缀和。换句话说，`prefix[i]` 是 `nums` 重新排列后下标从 `0` 到 `i` 的元素之和。`nums` 的 **分数** 是 `prefix` 数组中正整数的个数。

返回可以得到的最大分数。

 

**示例 1：**

```
输入：nums = [2,-1,0,1,-3,3,-3]
输出：6
解释：数组重排为 nums = [2,3,1,-1,-3,0,-3] 。
prefix = [2,5,6,5,2,2,-1] ，分数为 6 。
可以证明 6 是能够得到的最大分数。
```

**示例 2：**

```
输入：nums = [-2,-3,0]
输出：0
解释：不管怎么重排数组得到的分数都是 0 。
```

 

**提示：**

- `1 <= nums.length <= 105`
- `-106 <= nums[i] <= 106`



### 模拟/排序

```cpp
class Solution {
public:
    int maxScore(vector<int>& nums) {
        sort(nums.begin(),nums.end(),greater<int>());//从大到小排序
        long sum=0;
        int res=0;
        for(int num:nums)
        {
            sum+=num;
            if(sum>0) res++;
            else break;
        }
        return res;

    }
};
```

要想使得前缀和的正数最多，那么越小的数应该放在越后面，即应该从大到小排序。



---

## Medium 2588. 统计美丽子数组数目 5 points

给你一个下标从 **0** 开始的整数数组`nums` 。每次操作中，你可以：

- 选择两个满足 `0 <= i, j < nums.length` 的不同下标 `i` 和 `j` 。
- 选择一个非负整数 `k` ，满足 `nums[i]` 和 `nums[j]` 在二进制下的第 `k` 位（下标编号从 **0** 开始）是 `1` 。
- 将 `nums[i]` 和 `nums[j]` 都减去 `2k` 。

如果一个子数组内执行上述操作若干次后，该子数组可以变成一个全为 `0` 的数组，那么我们称它是一个 **美丽** 的子数组。

请你返回数组 `nums` 中 **美丽子数组** 的数目。

子数组是一个数组中一段连续 **非空** 的元素序列。

 

**示例 1：**

```
输入：nums = [4,3,1,2,4]
输出：2
解释：nums 中有 2 个美丽子数组：[4,3,1,2,4] 和 [4,3,1,2,4] 。
- 按照下述步骤，我们可以将子数组 [3,1,2] 中所有元素变成 0 ：
  - 选择 [3, 1, 2] 和 k = 1 。将 2 个数字都减去 21 ，子数组变成 [1, 1, 0] 。
  - 选择 [1, 1, 0] 和 k = 0 。将 2 个数字都减去 20 ，子数组变成 [0, 0, 0] 。
- 按照下述步骤，我们可以将子数组 [4,3,1,2,4] 中所有元素变成 0 ：
  - 选择 [4, 3, 1, 2, 4] 和 k = 2 。将 2 个数字都减去 22 ，子数组变成 [0, 3, 1, 2, 0] 。
  - 选择 [0, 3, 1, 2, 0] 和 k = 0 。将 2 个数字都减去 20 ，子数组变成 [0, 2, 0, 2, 0] 。
  - 选择 [0, 2, 0, 2, 0] 和 k = 1 。将 2 个数字都减去 21 ，子数组变成 [0, 0, 0, 0, 0] 。
```

**示例 2：**

```
输入：nums = [1,10,4]
输出：0
解释：nums 中没有任何美丽子数组。
```

 

**提示：**

- `1 <= nums.length <= 105`
- `0 <= nums[i] <= 106`



### 位运算/前缀和

```cpp
class Solution {
public:
    long long beautifulSubarrays(vector<int>& nums) {
        vector<int> presum(nums.size()+1);//定义前缀和存放容器
        long long res=0;
        unordered_map<int,int> same;
        same[0]++;//由于 presum[0]==0 是不进行下面运算的，所以要把这个提前加入其中，可以得到从索引 0 开始的子数组
        for(int i=1;i<nums.size()+1;i++)
        {
            presum[i]=presum[i-1] ^ nums[i-1];//计算异或前缀和
            res+=same[presum[i]];//计数
            same[presum[i]]++;//计数后再让该数字 ++ 
        }
        return res;
    }
};
```

主体思路在于：位运算中的异或运算（按位异或，相同则为 0 ，不同则为 1 ），这样是正好符合同时对于 i 和 j 位的数字减 1 的，其实从另一个角度理解就是子数组中的该位为 1 的次数为偶数次才可以，即子数组按位异或后应该 == 0。对于前缀异或和而言，只要记录前缀数组的异或和的值，一旦有 `异或和[i]==异或和[j]`，则子数组`[i,j]`为美丽子数组。

这个题也可以进一步优化不使用定义前缀和存放容器，直接用一个整型变量代替，因为前缀和已经存放到哈希表中了。

```cpp
class Solution {
public:
    long long beautifulSubarrays(vector<int>& nums) {
        int presum=0;
        long long res=0;
        unordered_map<int,int> same;
        same[0]++;
        for(int i=1;i<nums.size()+1;i++)
        {
            presum=presum ^ nums[i-1];
            res+=same[presum];
            same[presum]++;
        }
        return res;
    }
};
```



---

## Hard 2589. 完成所有任务的最少时间 6 points

- 你有一台电脑，它可以 **同时** 运行无数个任务。给你一个二维整数数组 `tasks` ，其中 `tasks[i] = [starti, endi, durationi]` 表示第 `i` 个任务需要在 **闭区间** 时间段 `[starti, endi]` 内运行 `durationi` 个整数时间点（但不需要连续）。

  当电脑需要运行任务时，你可以打开电脑，如果空闲时，你可以将电脑关闭。

  请你返回完成所有任务的情况下，电脑最少需要运行多少秒。

   

  **示例 1：**

  ```
  输入：tasks = [[2,3,1],[4,5,1],[1,5,2]]
  输出：2
  解释：
  - 第一个任务在闭区间 [2, 2] 运行。
  - 第二个任务在闭区间 [5, 5] 运行。
  - 第三个任务在闭区间 [2, 2] 和 [5, 5] 运行。
  电脑总共运行 2 个整数时间点。
  ```

  **示例 2：**

  ```
  输入：tasks = [[1,3,2],[2,5,3],[5,6,2]]
  输出：4
  解释：
  - 第一个任务在闭区间 [2, 3] 运行
  - 第二个任务在闭区间 [2, 3] 和 [5, 5] 运行。
  - 第三个任务在闭区间 [5, 6] 运行。
  电脑总共运行 4 个整数时间点。
  ```

   

  **提示：**

  - `1 <= tasks.length <= 2000`
  - `tasks[i].length == 3`
  - `1 <= starti, endi <= 2000`
  - `1 <= durationi <= endi - starti + 1 `

  

### 暴力/贪心

思路在于：额外定义一个运行时间数组，这个数组用来表示某个时间点电脑是否在运行，随后按结束时间进行排序，对于每一个程序，首先就目前已知的运行时间运行所有的内容，随后判断其是否运行完成，如果未运行完成则从结束时间开始向前添加运行时间。**值得注意的是应该从结束时间进行添加运行时间，而不是从初始时间开始。**

```cpp
class Solution {
    bool run[2001];//因为始末时间为1-2000，使得末尾索引为2000，用于记录运行时间
public:
    int findMinimumTime(vector<vector<int>> &tasks) {
        sort(tasks.begin(), tasks.end(), [](auto &a, auto &b) {
            return a[1] < b[1];
        });//根据结束时间进行排序
        int ans = 0;
        for (auto &t : tasks) {
            int start = t[0], end = t[1], d = t[2];
            for (int i = start; i <= end; ++i)
                d -= run[i]; // 去掉运行中的时间点，表明该时间点上一定运行
            for (int i = end; d > 0; --i) // 从结束时间开始进行遍历直到运行时间为0
                if (!run[i]) {//如果没有运行则该时间需要运行
                    run[i] = true;
                    --d;
                    ++ans;
                }
        }
        return ans;
    }
};
```

这个题还有更加优化的做法：线段树/(栈+二分)/差分约束 ，详见：

> 灵茶山艾府：https://leetcode.cn/problems/minimum-time-to-complete-all-tasks/solutions/2163130/tan-xin-pythonjavacgo-by-endlesscheng-w3k3/
>
> TsReaper：https://leetcode.cn/problems/minimum-time-to-complete-all-tasks/solutions/2163154/chai-fen-yue-shu-by-tsreaper-swhm/