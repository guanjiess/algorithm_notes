记忆化搜索，简单来说就是把回溯算法的计算结果记录了下来，用以解决动态规划问题。

大多数情况下，动态规划问题都可以用记忆化搜索解决。

## 记忆化搜索

1. 简单来说，就是通过回溯暴力穷举所有答案，把中途已经计算的答案保存下来，以实现剪枝的效果，减少递归的次数。还是个递归，只不过多了一步保存计算结果的过程。

   <img src="E:\master2\coding_notes\DSA\algorithm-dp-memoization.assets\1715238910154.png" alt=" " style="zoom:67%;" />

   <img src="E:\master2\coding_notes\DSA\algorithm-dp-memoization.assets\1715238944575.png" alt="1715238944575" style="zoom:67%;" />

2. 相比于动态规划DP，回溯or**记忆化搜索从自顶向下**的视角解决问题，**动态规划从自底向上**的视角解决问题。

3. **时间复杂度**：状态个数*单个状态计算时间

==问题==

1. 确定入参和返回值
2. 递归边界和入口
3. 递归到哪里

##从记忆化搜索翻译DP

1. 把dfs改为dp数组
2. 把递归改为循环
3. 递归边界改为dp数组的初始值

如

```C++
// 会超时的递归写法
class Solution {
public:
    int jewelleryValue(vector<vector<int>> &grid) {
        function<int(int, int)> dfs = [&](int i, int j) -> int {
            if (i < 0 || j < 0)
                return 0;
            return max(dfs(i, j - 1), dfs(i - 1, j)) + grid[i][j];
        };
        return dfs(grid.size() - 1, grid[0].size() - 1);
    }
};

```

改为

```C++
class Solution {
public:
    int jewelleryValue(vector<vector<int>> &grid) {
        int m = grid.size(), n = grid[0].size(), f[m + 1][n + 1];
        memset(f, 0, sizeof(f));
        for (int i = 0; i < m; ++i)
            for (int j = 0; j < n; ++j)
                f[i + 1][j + 1] = max(f[i + 1][j], f[i][j + 1]) + grid[i][j];
        return f[m][n];
    }
};
```

注意，在动态规划方法中，数组的维度做了一定修改，目的是避免边界问题，详解见https://leetcode.cn/problems/li-wu-de-zui-da-jie-zhi-lcof/solutions/2153802/jiao-ni-yi-bu-bu-si-kao-dpcong-hui-su-da-epvl/



## tricks

<img src="E:\master2\coding_notes\DSA\algorithm-dp-memoization.assets\1715239298333.png" alt="1715239298333" style="zoom:67%;" />

- i=0  i = 1时，i-1  i-2小于0，需要特判。往前面插两个空，省去了特判的过程，对结果也没有任何影响。
  $$
  dfs(i)=max(dfs(i-1),dfs(i-2)+nums[i])\rightarrow f[i]=max(f[i-1],f[i-2]+nums[i])
  $$
  































