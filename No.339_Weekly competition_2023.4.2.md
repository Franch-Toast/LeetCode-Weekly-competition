# No.339 Weekly competition

## Easy 2609. 最长平衡子字符串 3 points

给你一个仅由 `0` 和 `1` 组成的二进制字符串 `s` 。 

如果子字符串中 **所有的** `0` **都在** `1` **之前** 且其中 `0` 的数量等于 `1` 的数量，则认为 `s` 的这个子字符串是平衡子字符串。请注意，空子字符串也视作平衡子字符串。 

返回 `s` 中最长的平衡子字符串长度。

子字符串是字符串中的一个连续字符序列。

 

**示例 1：**

```
输入：s = "01000111"
输出：6
解释：最长的平衡子字符串是 "000111" ，长度为 6 。
```

**示例 2：**

```
输入：s = "00111"
输出：4
解释：最长的平衡子字符串是 "0011" ，长度为  4 。
```

**示例 3：**

```
输入：s = "111"
输出：0
解释：除了空子字符串之外不存在其他平衡子字符串，所以答案为 0 。
```

 

**提示：**

- `1 <= s.length <= 50`
- `'0' <= s[i] <= '1'`



### 模拟

在循环中维护变量进行比较。

```cpp
class Solution {
public:
    int findTheLongestBalancedSubstring(string s) {
        if(s.size()==0) return 0;
        int ans=0;
        int i=0;
        while(i<s.size())
        {
            int zero=0;//用于记录这一组的0和1的个数
            int one=0;
            while(i<s.size() && s[i]=='0')
            {
                zero++;
                i++;
            }
            while(i<s.size() && s[i]=='1')
            {
                one++;
                i++;
            }
            ans=max(ans,min(zero,one)*2);
        }
        return ans;
    }
};
```

也可以不断遍历直到出现不同的字符，当当前字符为1时，则进行一次最大判断（必须要先0后1）。

```cpp
class Solution {
public:
    int findTheLongestBalancedSubstring(string s) {
        int ans = 0, pre = 0, cur = 0, n = s.length();
        for (int i = 0; i < n; ++i) {
            ++cur;
            if (i == s.length() - 1 || s[i] != s[i + 1]) {//连续中断或遍历结束
                if (s[i] == '1')//只有在当前为1连续中断时判断
                    ans = max(ans, min(pre, cur) * 2);
                pre = cur;
                cur = 0;
            }
        }
        return ans;
    }
};
```



---

## Medium 2610. 转换二维数组 4 points

给你一个整数数组 `nums` 。请你创建一个满足以下条件的二维数组：

- 二维数组应该 **只** 包含数组 `nums` 中的元素。
- 二维数组中的每一行都包含 **不同** 的整数。
- 二维数组的行数应尽可能 **少** 。

返回结果数组。如果存在多种答案，则返回其中任何一种。

请注意，二维数组的每一行上可以存在不同数量的元素。

 

**示例 1：**

```
输入：nums = [1,3,4,1,2,3,1]
输出：[[1,3,4,2],[1,3],[1]]
解释：根据题目要求可以创建包含以下几行元素的二维数组：
- 1,3,4,2
- 1,3
- 1
nums 中的所有元素都有用到，并且每一行都由不同的整数组成，所以这是一个符合题目要求的答案。
可以证明无法创建少于三行且符合题目要求的二维数组。
```

**示例 2：**

```
输入：nums = [1,2,3,4]
输出：[[4,3,2,1]]
解释：nums 中的所有元素都不同，所以我们可以将其全部保存在二维数组中的第一行。
```

 

**提示：**

- `1 <= nums.length <= 200`
- `1 <= nums[i] <= nums.length`



### 哈希表/模拟

使用哈希表或数组计数，每一次遍历一遍，如果当前的数字的数量不为零，则添加到数组中，并将数字统计减1，直到所有的数字统计都为0。

```cpp
class Solution {
public:
    vector<vector<int>> findMatrix(vector<int>& nums) {
        int n=nums.size();
        int list[n+1];
        memset(list,0,sizeof(list));
        
        for(int num:nums) list[num]++;//计数
        
        
        vector<vector<int>> res;
        while(n)
        {
            vector<int> temp;
            for(int i=1;i<nums.size()+1;i++)
            {
                if(list[i]!=0)
                {
                    temp.push_back(i);
                    list[i]--;
                    n--;
                }
            }
            res.push_back(temp);
        }
        
        return res;
    }
};
```



---

## Medium 2611. 老鼠和奶酪 5 points

有两只老鼠和 `n` 块不同类型的奶酪，每块奶酪都只能被其中一只老鼠吃掉。

下标为 `i` 处的奶酪被吃掉的得分为：

- 如果第一只老鼠吃掉，则得分为 `reward1[i]` 。
- 如果第二只老鼠吃掉，则得分为 `reward2[i]` 。

给你一个正整数数组 `reward1` ，一个正整数数组 `reward2` ，和一个非负整数 `k` 。

请你返回第一只老鼠恰好吃掉 `k` 块奶酪的情况下，**最大** 得分为多少。

 

**示例 1：**

```
输入：reward1 = [1,1,3,4], reward2 = [4,4,1,1], k = 2
输出：15
解释：这个例子中，第一只老鼠吃掉第 2 和 3 块奶酪（下标从 0 开始），第二只老鼠吃掉第 0 和 1 块奶酪。
总得分为 4 + 4 + 3 + 4 = 15 。
15 是最高得分。
```

**示例 2：**

```
输入：reward1 = [1,1], reward2 = [1,1], k = 2
输出：2
解释：这个例子中，第一只老鼠吃掉第 0 和 1 块奶酪（下标从 0 开始），第二只老鼠不吃任何奶酪。
总得分为 1 + 1 = 2 。
2 是最高得分。
```

 

**提示：**

- `1 <= n == reward1.length == reward2.length <= 105`
- `1 <= reward1[i], reward2[i] <= 1000`
- `0 <= k <= n`



### 贪心

贪心的思路：如果老鼠1吃某块奶酪的得分比老鼠2吃的得分高，则这块奶酪给老鼠1吃是更好的选择，当老鼠1吃奶酪的数量有限的情况下，老鼠1优先吃两者得分较大的奶酪。

根据以上思想，计算两者的得分差并按从大到小排序，老鼠1会优先吃得分差大的奶酪（如果奶酪数量未够，即使得分差为负数依然会选择吃），直到奶酪数量够了，则会给老鼠二吃。

```cpp
class Solution {
public:
    int miceAndCheese(vector<int>& reward1, vector<int>& reward2, int k) {
        
        int res=0;
        vector<pair<int,int>> ans;
        for(int i=0;i<reward1.size();i++)
        {
            ans.push_back({reward1[i]-reward2[i],i});//记录得分差和相应的奶酪序号
        }
        sort(ans.begin(),ans.end(),greater());//根据得分差进行排序
        for(auto [_,i]:ans)
        {   
            if(k>0)//老鼠1还未吃够，会一直按顺序给老鼠1吃
            {
                res+=reward1[i];
                k--;
            }
            else res+=reward2[i];  
        }
        return res;
        
    }
};
```

灵神也是同样的思路，然而他是先将所有的奶酪给老鼠2吃，再计算出排序后的k个奶酪给老鼠1吃会比给老鼠2吃多多少或少多少。

```cpp
class Solution {
public:
    int miceAndCheese(vector<int> &r1, vector<int> &r2, int k) {
        for (int i = 0; i < r1.size(); ++i)
            r1[i] -= r2[i];
        nth_element(r1.begin(), r1.end() - k, r1.end());//定位从最后向前的第k个升序排序
        return accumulate(r2.begin(), r2.end(), 0) +
               accumulate(r1.end() - k, r1.end(), 0);//这部分给老鼠1吃会增加减少多少分数
    }
};
```



---

## Hard 2612. 最少翻转操作数 7 points

给你一个整数 `n` 和一个在范围 `[0, n - 1]` 以内的整数 `p` ，它们表示一个长度为 `n` 且下标从 **0** 开始的数组 `arr` ，数组中除了下标为 `p` 处是 `1` 以外，其他所有数都是 `0` 。

同时给你一个整数数组 `banned` ，它包含数组中的一些位置。`banned` 中第 **i** 个位置表示 `arr[banned[i]] = 0` ，题目保证 `banned[i] != p` 。

你可以对 `arr` 进行 **若干次** 操作。一次操作中，你选择大小为 `k` 的一个 **子数组** ，并将它 **翻转** 。在任何一次翻转操作后，你都需要确保 `arr` 中唯一的 `1` 不会到达任何 `banned` 中的位置。换句话说，`arr[banned[i]]` 始终 **保持** `0` 。

请你返回一个数组 `ans` ，对于 `[0, n - 1]` 之间的任意下标 `i` ，`ans[i]` 是将 `1` 放到位置 `i` 处的 **最少** 翻转操作次数，如果无法放到位置 `i` 处，此数为 `-1` 。

- **子数组** 指的是一个数组里一段连续 **非空** 的元素序列。
- 对于所有的 `i` ，`ans[i]` 相互之间独立计算。
- 将一个数组中的元素 **翻转** 指的是将数组中的值变成 **相反顺序** 。

 

**示例 1：**

```
输入：n = 4, p = 0, banned = [1,2], k = 4
输出：[0,-1,-1,1]
解释：k = 4，所以只有一种可行的翻转操作，就是将整个数组翻转。一开始 1 在位置 0 处，所以将它翻转到位置 0 处需要的操作数为 0 。
我们不能将 1 翻转到 banned 中的位置，所以位置 1 和 2 处的答案都是 -1 。
通过一次翻转操作，可以将 1 放到位置 3 处，所以位置 3 的答案是 1 。
```

**示例 2：**

```
输入：n = 5, p = 0, banned = [2,4], k = 3
输出：[0,-1,-1,-1,-1]
解释：这个例子中 1 一开始在位置 0 处，所以此下标的答案为 0 。
翻转的子数组长度为 k = 3 ，1 此时在位置 0 处，所以我们可以翻转子数组 [0, 2]，但翻转后的下标 2 在 banned 中，所以不能执行此操作。
由于 1 没法离开位置 0 ，所以其他位置的答案都是 -1 。
```

**示例 3：**

```
输入：n = 4, p = 2, banned = [0,1,3], k = 1
输出：[-1,-1,0,-1]
解释：这个例子中，我们只能对长度为 1 的子数组执行翻转操作，所以 1 无法离开初始位置。
```

 

**提示：**

- `1 <= n <= 105`
- `0 <= p <= n - 1`
- `0 <= banned.length <= n - 1`
- `0 <= banned[i] <= n - 1`
- `1 <= k <= n `
- `banned[i] != p`
- `banned` 中的值 **互不相同** 。



### BFS+平衡树

这题较为复杂，细节很多，这里推荐看灵神的视频，不过多解读。

先通过找规律找到翻转的左边界和右边界以及反转的位置规律是一个公差为二的等差数列，再用两颗平衡树维护下标索引，将访问过的删除，整体使用BFS搜索。

```cpp
class Solution {
public:
    vector<int> minReverseOperations(int n, int p, vector<int> &banned, int k) {
        unordered_set<int> ban{banned.begin(), banned.end()};
        set<int> sets[2];
        for (int i = 0; i < n; ++i)
            if (i != p && !ban.count(i))
                sets[i % 2].insert(i);
        sets[0].insert(n);
        sets[1].insert(n); // 哨兵

        vector<int> ans(n, -1);
        vector<int> q = {p};
        for (int step = 0; !q.empty(); ++step) {
            vector<int> nq;
            for (int i: q) {
                ans[i] = step;
                // 从 mn 到 mx 的所有位置都可以翻转到
                int mn = max(i - k + 1, k - i - 1);
                int mx = min(i + k - 1, n * 2 - k - i - 1);
                auto &s = sets[mn % 2];
                for (auto it = s.lower_bound(mn); *it <= mx; it = s.erase(it))
                    nq.push_back(*it);
            }
            q = move(nq);
        }
        return ans;
    }
};

```

> 灵茶山艾府：https://leetcode.cn/problems/minimum-reverse-operations/solutions/2204092/liang-chong-zuo-fa-ping-heng-shu-bing-ch-vr0z/