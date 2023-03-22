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

