

# No.99 Biweekly competition

## Easy 6312. 最小和分割 3 points

给你一个正整数 `num` ，请你将它分割成两个非负整数 `num1` 和 `num2` ，满足：

- `num1` 和 `num2` 直接连起来，得到 `num` 各数位的一个排列

   	换句话说，`num1` 和`num2`中所有数字出现的次数之和等于`num` 中所有数字出现的次数。

- `num1` 和 `num2` 可以包含前导 0 。

请你返回 `num1` 和 `num2` 可以得到的和的 **最小** 值。

**注意：**

- `num` 保证没有前导 0 。
- `num1` 和 `num2` 中数位顺序可以与 `num` 中数位顺序不同。

 

**示例 1：**

```
输入：num = 4325
输出：59
解释：我们可以将 4325 分割成 num1 = 24 和 num2 = 35 ，和为 59 ，59 是最小和。
```

**示例 2：**

```
输入：num = 687
输出：75
解释：我们可以将 687 分割成 num1 = 68 和 num2 = 7 ，和为最优值 75 。
```

 

**提示：**

- `10 <= num <= 109`



### 哈希表+优先分配

```cpp
class Solution {
public:
    int splitNum(int num) {
        map<int,int> cur;
        while(num!=0)//统计各数字的出现了几次
        {
            cur[num%10]++;
            num=num/10;
        }
        //接下来进行数字的分配，优先分配小的数
        int num1=0;
        int num2=0;
        for(auto[key,value]:cur)
        {
            for(int i=0;i<value;i++)
            {
                num1<num2?num1=num1*10+key:num2=num2*10+key;
            }
        }
        return num1+num2;
    }
};
```

实际原理：**把小的排在高位更优，大的排在低位更优**。我使用的方法是通过哈希表统计数字的出现次数，再进行均匀分配，优先分配小的数字。

当然也可以转换为字符串后根据奇偶位来进行分配，和上面一种做法类似。

### 转换字符串+奇偶位优先分配

```cpp
class Solution {
public:
    int splitNum(int num) {
        string s = to_string(num);
        sort(s.begin(), s.end());
        int a[2]{};
        for (int i = 0; i < s.length(); ++i)
            a[i % 2] = a[i % 2] * 10 + s[i] - '0'; // 按照奇偶下标分组
        return a[0] + a[1];
    }
};
```





---

## Medium 6311. 统计染色格子数 4 points

- 有一个无穷大的二维网格图，一开始所有格子都未染色。给你一个正整数 `n` ，表示你需要执行以下步骤 `n` 分钟：

  - 第一分钟，将 **任一** 格子染成蓝色。
  - 之后的每一分钟，将与蓝色格子相邻的 **所有** 未染色格子染成蓝色。

  下图分别是 1、2、3 分钟后的网格图。

  ![img](https://assets.leetcode.com/uploads/2023/01/10/example-copy-2.png)

  请你返回 `n` 分钟之后 **被染色的格子** 数目。

   

  **示例 1：**

  ```
  输入：n = 1
  输出：1
  解释：1 分钟后，只有 1 个蓝色的格子，所以返回 1 。
  ```

  **示例 2：**

  ```
  输入：n = 2
  输出：5
  解释：2 分钟后，有 4 个在边缘的蓝色格子和 1 个在中间的蓝色格子，所以返回 5 。
  ```

   

  **提示：**

  - `1 <= n <= 105`



### 数学/找规律

```cpp
class Solution {
public:
    long long coloredCells(int n) {
        long long res=1;
        for(int i=1;i<n;i++)
        {
            res=res+4*i;
        }
        return res;
    }
};
```

实际上就是一个找规律问题，找到相应的递推关系就可以了。



当然这个问题也可以由递推问题变成项数解析式：

$$ f(n)=f(n-1)+4n $$可以计算得到通项公式：$f(n)=2n(n-1)+1 \ \  ,n>1$





---

## Medium 6313. 统计将重叠区间合并成组的方案数 5 points

给你一个二维整数数组 `ranges` ，其中 `ranges[i] = [starti, endi]` 表示 `starti` 到 `endi` 之间（包括二者）的所有整数都包含在第 `i` 个区间中。

你需要将 `ranges` 分成 **两个** 组（可以为空），满足：

- 每个区间只属于一个组。
- 两个有 **交集** 的区间必须在 **同一个** 组内。

如果两个区间有至少 **一个** 公共整数，那么这两个区间是 **有交集** 的。

- 比方说，区间 `[1, 3]` 和 `[2, 5]` 有交集，因为 `2` 和 `3` 在两个区间中都被包含。

请你返回将 `ranges` 划分成两个组的 **总方案数** 。由于答案可能很大，将它对 `109 + 7` **取余** 后返回。

 

**示例 1：**

```
输入：ranges = [[6,10],[5,15]]
输出：2
解释：
两个区间有交集，所以它们必须在同一个组内。
所以有两种方案：
- 将两个区间都放在第 1 个组中。
- 将两个区间都放在第 2 个组中。
```

**示例 2：**

```
输入：ranges = [[1,3],[10,20],[2,5],[4,8]]
输出：4
解释：
区间 [1,3] 和 [2,5] 有交集，所以它们必须在同一个组中。
同理，区间 [2,5] 和 [4,8] 也有交集，所以它们也必须在同一个组中。
所以总共有 4 种分组方案：
- 所有区间都在第 1 组。
- 所有区间都在第 2 组。
- 区间 [1,3] ，[2,5] 和 [4,8] 在第 1 个组中，[10,20] 在第 2 个组中。
- 区间 [1,3] ，[2,5] 和 [4,8] 在第 2 个组中，[10,20] 在第 1 个组中。
```

 

**提示：**

- `1 <= ranges.length <= 105`
- `ranges[i].length == 2`
- `0 <= starti <= endi <= 109`



### DP

```cpp
class Solution {
public:
    int countWays(vector<vector<int>>& ranges) {
        int count=ranges.size();
        vector<long long> res(count,-1);
        sort(ranges.begin(),ranges.end());
        res[0]=2;
        for(int i=1;i<count;i++)
        {
            if(ranges[i][0]<=ranges[i-1][1])//有重叠情况，两者必须在一个组内
            {
                res[i]=res[i-1];
                ranges[i][1]=max(ranges[i-1][1],ranges[i][1]);
            }
            else res[i]=(res[i-1]*2)%(int)(pow(10,9)+7);//可以替换为 const int MOD = 1e9 + 7;
        }
        return (*(res.end()-1));
    }
};
```

我的这种做法可以参考 [452. 用最少数量的箭引爆气球](https://leetcode.cn/problems/minimum-number-of-arrows-to-burst-balloons/ )。



或有如下的做法：其实本质是一样的。

```cpp
class Solution {
    const int MOD = 1e9 + 7;
public:
    int countWays(vector<vector<int>> &ranges) {
        sort(ranges.begin(), ranges.end(), [](auto &a, auto &b) {
            return a[0] < b[0];
        });
        int ans = 2, max_r = ranges[0][1];
        for (auto &p : ranges) {
            if (p[0] > max_r) // 产生了一个新的集合
                ans = ans * 2 % MOD;
            max_r = max(max_r, p[1]);
        }
        return ans;
    }
};
```





---

## Hard 6314. 统计可能的树根数目 6 points

Alice 有一棵 `n` 个节点的树，节点编号为 `0` 到 `n - 1` 。树用一个长度为 `n - 1` 的二维整数数组 `edges` 表示，其中 `edges[i] = [ai, bi]` ，表示树中节点 `ai` 和 `bi` 之间有一条边。

Alice 想要 Bob 找到这棵树的根。她允许 Bob 对这棵树进行若干次 **猜测** 。每一次猜测，Bob 做如下事情：

- 选择两个 **不相等** 的整数 `u` 和 `v` ，且树中必须存在边 `[u, v]` 。
- Bob 猜测树中 `u` 是 `v` 的 **父节点** 。

Bob 的猜测用二维整数数组 `guesses` 表示，其中 `guesses[j] = [uj, vj]` 表示 Bob 猜 `uj` 是 `vj` 的父节点。

Alice 非常懒，她不想逐个回答 Bob 的猜测，只告诉 Bob 这些猜测里面 **至少** 有 `k` 个猜测的结果为 `true` 。

给你二维整数数组 `edges` ，Bob 的所有猜测和整数 `k` ，请你返回可能成为树根的 **节点数目** 。如果没有这样的树，则返回 `0`。

 

**示例 1：**

![img](https://assets.leetcode.com/uploads/2022/12/19/ex-1.png)

```
输入：edges = [[0,1],[1,2],[1,3],[4,2]], guesses = [[1,3],[0,1],[1,0],[2,4]], k = 3
输出：3
解释：
根为节点 0 ，正确的猜测为 [1,3], [0,1], [2,4]
根为节点 1 ，正确的猜测为 [1,3], [1,0], [2,4]
根为节点 2 ，正确的猜测为 [1,3], [1,0], [2,4]
根为节点 3 ，正确的猜测为 [1,0], [2,4]
根为节点 4 ，正确的猜测为 [1,3], [1,0]
节点 0 ，1 或 2 为根时，可以得到 3 个正确的猜测。
```

**示例 2：**

![img](https://assets.leetcode.com/uploads/2022/12/19/ex-2.png)

```
输入：edges = [[0,1],[1,2],[2,3],[3,4]], guesses = [[1,0],[3,4],[2,1],[3,2]], k = 1
输出：5
解释：
根为节点 0 ，正确的猜测为 [3,4]
根为节点 1 ，正确的猜测为 [1,0], [3,4]
根为节点 2 ，正确的猜测为 [1,0], [2,1], [3,4]
根为节点 3 ，正确的猜测为 [1,0], [2,1], [3,2], [3,4]
根为节点 4 ，正确的猜测为 [1,0], [2,1], [3,2]
任何节点为根，都至少有 1 个正确的猜测。
```

 

**提示：**

- `edges.length == n - 1`
- `2 <= n <= 105`
- `1 <= guesses.length <= 105`
- `0 <= ai, bi, uj, vj <= n - 1`
- `ai != bi`
- `uj != vj`
- `edges` 表示一棵有效的树。
- `guesses[j]` 是树中的一条边。
- `0 <= k <= guesses.length`



### 哈希/DFS/DP

```cpp
class Solution {
public:
    int rootCount(vector<vector<int>> &edges, vector<vector<int>> &guesses, int k) {
        vector<vector<int>> g(edges.size() + 1);
        for (auto &e : edges) {
            int x = e[0], y = e[1];
            g[x].push_back(y);
            g[y].push_back(x); // 建图
        }

        unordered_set<long> s;
        for (auto &e : guesses) // guesses 转成哈希表
            s.insert((long) e[0] << 32 | e[1]); // 两个 4 字节数压缩成一个 8 字节数

        int ans = 0, cnt0 = 0;//cnt0是猜对的次数
        function<void(int, int)> dfs = [&](int x, int fa) {//x:当前节点,fa:父节点
            for (int y : g[x])//遍历当前节点的所有连接节点
                if (y != fa) {//如果当前连接节点不是父节点
                    cnt0 += s.count((long) x << 32 | y); // 在guesses中查找，存在以 0 为根时，猜对了则+1
                    dfs(y, x);//将该连接节点作为研究对象，原研究节点作为父节点来遍历，实际上就是在做递归DFS，不断向连接节点深处递归
                }
        };
        dfs(0, -1);//0作为根节点，它是没有父节点的，所以令fa=-1，使得y != fa，这里是在做第一次dfs遍历树节点，先得到以0为根节点时guesses猜对的个数

        function<void(int, int, int)> reroot = [&](int x, int fa, int cnt) {
            ans += cnt >= k; // 此时 cnt 就是以 x 为根时的猜对次数
            for (int y : g[x])
                if (y != fa) {//换根
                    reroot(y, x, cnt
                                 - s.count((long) x << 32 | y) //  （如果存在，那么）原来是对的，现在错了，需要去除
                                 + s.count((long) y << 32 | x)); // （如果存在，那么）原来是错的，现在对了，需要加入
                }
        };//同样是一场DFS
        reroot(0, -1, cnt0);
        return ans;
    }
};

```

这题值得注意的一点是，当我（假设）确定一个结点为根节点的时候，这颗树根据 edgs 就已经被确定了，例如：设 0 为根节点，那么与连接的 1 、2 一定是它的子节点，这就意味着与 1 或 2 各自连接的其他的节点一定是他们各自的子节点，而不可能父节点，因为这是一颗树，一个与某一个节点连接的结点中一定只有一个父节点，其他的都是子节点。

这里定义的两个函数都是在做DFS递推的工作，dfs 函数在递推过程中确认猜测正确的次数，后续的 reroot 换根函数在递推过程中修改cnt0 ，减去原本可能存在的（原先正确的）节点对，加上（现在正确的）节点对，前后只会有 1 的差别。



> 此题的答案参考：
>
> 作者：灵茶山艾府
> 链接：https://leetcode.cn/problems/count-number-of-possible-root-nodes/solutions/2147714/huan-gen-dppythonjavacgo-by-endlesscheng-ccwy/

