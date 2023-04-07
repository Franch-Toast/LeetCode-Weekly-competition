

# No.101 Biweekly competition

## Easy 2605. 从两个数字数组里生成最小数字 3 points

给你两个只包含 1 到 9 之间数字的数组 `nums1` 和 `nums2` ，每个数组中的元素 **互不相同** ，请你返回 **最小** 的数字，两个数组都 **至少** 包含这个数字的某个数位。

 

**示例 1：**

```
输入：nums1 = [4,1,3], nums2 = [5,7]
输出：15
解释：数字 15 的数位 1 在 nums1 中出现，数位 5 在 nums2 中出现。15 是我们能得到的最小数字。
```

**示例 2：**

```
输入：nums1 = [3,5,2,6], nums2 = [3,1,7]
输出：3
解释：数字 3 的数位 3 在两个数组中都出现了。
```

 

**提示：**

- `1 <= nums1.length, nums2.length <= 9`
- `1 <= nums1[i], nums2[i] <= 9`
- 每个数组中，元素 **互不相同** 。



### 哈希表

哈希表存入数字，如果有重复的数字则直接输出，否则选择二者的最小进行最小的组合输出。

```cpp
class Solution {
public:
    int minNumber(vector<int>& nums1, vector<int>& nums2) {
        unordered_set<int> list;
        
        int i=10;
        for(int num:nums1)
        {
            i=min(i,num);
            list.insert(num);
        }
        
        sort(nums2.begin(),nums2.end());//这里需要进行排序，因为可能出现多个重复数字，但是题目要求最小的数字输出
        int j=10;
        for(int num:nums2)
        {
            if(list.count(num)) return num;
            j=min(j,num);
            
        }
        
        return min(i,j)*10+max(i,j);
    }
};
```



---

## Medium 2606. 找到最大开销的子字符串 4 points

给你一个字符串 `s` ，一个字符 **互不相同** 的字符串 `chars` 和一个长度与 `chars` 相同的整数数组 `vals` 。

**子字符串的开销** 是一个子字符串中所有字符对应价值之和。空字符串的开销是 `0` 。

**字符的价值** 定义如下：

- 如果字符不在字符串 ` chars`中，那么它的价值是它在字母表中的位置（下标从1 开始）。
  - 比方说，`'a'` 的价值为 `1` ，`'b'` 的价值为 `2` ，以此类推，`'z'` 的价值为 `26` 。
- 否则，如果这个字符在 `chars` 中的位置为 `i` ，那么它的价值就是 `vals[i]` 。

请你返回字符串 `s` 的所有子字符串中的最大开销。

 

**示例 1：**

```
输入：s = "adaa", chars = "d", vals = [-1000]
输出：2
解释：字符 "a" 和 "d" 的价值分别为 1 和 -1000 。
最大开销子字符串是 "aa" ，它的开销为 1 + 1 = 2 。
2 是最大开销。
```

**示例 2：**

```
输入：s = "abc", chars = "abc", vals = [-1,-1,-1]
输出：0
解释：字符 "a" ，"b" 和 "c" 的价值分别为 -1 ，-1 和 -1 。
最大开销子字符串是 "" ，它的开销为 0 。
0 是最大开销。
```

 

**提示：**

- `1 <= s.length <= 105`
- `s` 只包含小写英文字母。
- `1 <= chars.length <= 26`
- `chars` 只包含小写英文字母，且 **互不相同** 。
- `vals.length == chars.length`
- `-1000 <= vals[i] <= 1000`



### DP

使用DP思想，以每个位置作为结尾的子字符串的最大的开销作为dp的元素，判断加上该元素后与 0 的比较大小作为后一个位置结尾的子字符串的最大开销，这里无需与自己的开销（实际是与 0 比较），因为该花销一定是 <0 的，所以如果此前的花销已经 <0 了，那么重新开始的字符串一定不会是由该位置开始的。

```cpp
class Solution {
public:
    int maximumCostSubstring(string s, string chars, vector<int>& vals) {
        if(s.size()==0) return 0;
        vector<int> f(s.size()+1,0);//定义dp数组，规定以索引i结尾的子字符串的最大开销

        unordered_map<char,int> list;
        for(int i=0;i<chars.size();i++) list[chars[i]]=vals[i];
        
        //已知 f[i]=max(f[i-1]+(i的开销),0); 以 i 结尾的开销是以 i+1  结尾的开销 + 当前开销，除非该开销小于0。
        for(int i=1;i<=s.size();i++)
        {
            if(list.count(s[i-1]))
                f[i]=max(f[i-1]+list[s[i-1]],0);
            else
                f[i]=max(f[i-1]+s[i-1]-'a'+1,0);
        }
        return *max_element(f.begin(), f.end());//找到其中以某个索引为结尾的最大开销
    }
};
```





---

## Medium 2607. 使子数组元素和相等 5 points

给你一个下标从 **0** 开始的整数数组 `arr` 和一个整数 `k` 。数组 `arr` 是一个循环数组。换句话说，数组中的最后一个元素的下一个元素是数组中的第一个元素，数组中第一个元素的前一个元素是数组中的最后一个元素。

你可以执行下述运算任意次：

- 选中 `arr` 中任意一个元素，并使其值加上 `1` 或减去 `1` 。

执行运算使每个长度为 `k` 的 **子数组** 的元素总和都相等，返回所需要的最少运算次数。

**子数组** 是数组的一个连续部分。

 

**示例 1：**

```
输入：arr = [1,4,1,3], k = 2
输出：1
解释：在下标为 1 的元素那里执行一次运算，使其等于 3 。
执行运算后，数组变为 [1,3,1,3] 。
- 0 处起始的子数组为 [1, 3] ，元素总和为 4 
- 1 处起始的子数组为 [3, 1] ，元素总和为 4 
- 2 处起始的子数组为 [1, 3] ，元素总和为 4 
- 3 处起始的子数组为 [3, 1] ，元素总和为 4 
```

**示例 2：**

```
输入：arr = [2,5,5,7], k = 3
输出：5
解释：在下标为 0 的元素那里执行三次运算，使其等于 5 。在下标为 3 的元素那里执行两次运算，使其等于 5 。
执行运算后，数组变为 [5,5,5,5] 。
- 0 处起始的子数组为 [5, 5, 5] ，元素总和为 15
- 1 处起始的子数组为 [5, 5, 5] ，元素总和为 15
- 2 处起始的子数组为 [5, 5, 5] ，元素总和为 15
- 3 处起始的子数组为 [5, 5, 5] ，元素总和为 15
```

 

**提示：**

- `1 <= k <= arr.length <= 105`
- `1 <= arr[i] <= 109`



### 裴蜀定理

**一个循环数组如果既有周期 n，又有周期 k，则必然有周期 gcd⁡(n,k)。**

还有值得注意的是这是个中位数贪心，即同一组的元素向中位数的方向变得一致是操作数量最少的。

```cpp
class Solution {
public:
    long long makeSubKSumEqual(vector<int> &arr, int k) {
        int n = arr.size();
        k = gcd(k, n);
        vector<vector<int>> g(k);
        for (int i = 0; i < n; ++i)
            g[i % k].push_back(arr[i]);//根据裴蜀定理分组

        long long ans = 0;
        for (auto &b: g) {
            nth_element(b.begin(), b.begin() + b.size() / 2, b.end());//仅把中位数的那个元素放到它该在的位置上
            for (int x: b)
                ans += abs(x - b[b.size() / 2]);//累加计算每个元素的操作次数
        }
        return ans;
    }
};
```

建议听灵茶山艾府的讲解，会更加清晰：

> https://leetcode.cn/problems/make-k-subarray-sums-equal/solutions/2203591/zhuan-huan-zhong-wei-shu-tan-xin-pei-shu-4dut/



---

## Hard 2608. 图中的最短环 6 points

现有一个含 `n` 个顶点的 **双向** 图，每个顶点按从 `0` 到 `n - 1` 标记。图中的边由二维整数数组 `edges` 表示，其中 `edges[i] = [ui, vi]` 表示顶点 `ui` 和 `vi` 之间存在一条边。每对顶点最多通过一条边连接，并且不存在与自身相连的顶点。

返回图中 **最短** 环的长度。如果不存在环，则返回 `-1` 。

**环** 是指以同一节点开始和结束，并且路径中的每条边仅使用一次。

 

**示例 1：**

![img](https://assets.leetcode.com/uploads/2023/01/04/cropped.png)

```
输入：n = 7, edges = [[0,1],[1,2],[2,0],[3,4],[4,5],[5,6],[6,3]]
输出：3
解释：长度最小的循环是：0 -> 1 -> 2 -> 0 
```

**示例 2：**

![img](https://assets.leetcode.com/uploads/2023/01/04/croppedagin.png)

```
输入：n = 4, edges = [[0,1],[0,2]]
输出：-1
解释：图中不存在循环
```

 

**提示：**

- `2 <= n <= 1000`
- `1 <= edges.length <= 1000`
- `edges[i].length == 2`
- `0 <= ui, vi < n`
- `ui != vi`
- 不存在重复的边



### BFS

使用BFS暴力搜索每一个点作为起点时的最小环。

值得注意的是在BFS过程中，**由于不是树形结构，如何来判断某一个结点连接的另一个结点是否之前就已经遍历过了（当前的这个结点是由这个前驱结点入栈的）**？这里使用的方法是==将这个结点入栈的同时将其前驱结点一起入栈==，进行一个`if (y != pre)`的判断，满足这个结点并非是前驱结点，而是后续的结点。

```cpp
class Solution {
public:
    int findShortestCycle(int n, vector<vector<int>> &edges) {
        vector<vector<int>> g(n);
        for (auto &e: edges) {
            int x = e[0], y = e[1];
            g[x].push_back(y);
            g[y].push_back(x); // 建一个双向图
        }

        int dis[n]; // dis[i] 表示从 start 到 i 的最短路长度
        auto bfs = [&](int start) -> int {
            int ans = INT_MAX;
            memset(dis, -1, sizeof(dis));
            dis[start] = 0;
            queue<pair<int, int>> q;//入栈元素为 <元素，它的前驱元素>
            q.emplace(start, -1);//start元素及其前驱元素入队（无）
            while (!q.empty()) {
                auto [x, fa] = q.front();
                q.pop();
                for (int y: g[x])
                    if (dis[y] < 0) { // 因为dis[]记录start到i的最短路，所以小于0时表示第一次遇到
                        dis[y] = dis[x] + 1;//y 由 x 而来，记录。
                        q.emplace(y, x);//入栈
                    } else if (y != fa) // x的后驱连接元素不是它的前驱元素，证明是第二次遇到
                        ans = min(ans, dis[x] + dis[y] + 1);//找到了有个环，用ans来维护最短的换
            }
            return ans;
        };
        int ans = INT_MAX;
        for (int i = 0; i < n; ++i) // 枚举每个起点跑 BFS
            ans = min(ans, bfs(i));
        return ans < INT_MAX ? ans : -1;
    }
};

```



> 灵茶山艾府：https://leetcode.cn/problems/shortest-cycle-in-a-graph/solutions/2203585/yi-tu-miao-dong-mei-ju-qi-dian-pao-bfspy-ntck/