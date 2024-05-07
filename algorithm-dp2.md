时间复杂度的计算：状态个数*单个状态的计算时间



##思维模式

自底向上计算子问题。把问题拆分为和**原问题相似、但是规模更小**的子问题。从两个方向思考问题

1. 选或不选
2. 枚举选哪个



## 记忆化和DP

==记忆化==

记忆化memo数组维度由递归参数而定。比如dfs(int r, int c)，显然就要创建一个二维的memo数组保存。



==由递归翻译DP==

1. 把dfs改成dp数组
2. 把递归改成循环，每个参数意味着一层循环
3. 递归边界即为dp数组的初始值



**问题**

1、递归翻译成递推的dp时，循环的顺序怎么确定？正序还是逆序遍历？

<img src="E:\master2\coding_notes\DSA\algorithm-dp2.assets\1713435024479.png" alt="1713435024479" style="zoom: 67%;" />

以https://leetcode.cn/problems/minimum-path-cost-in-a-grid/solutions/2536856/jiao-ni-yi-bu-bu-si-kao-dong-tai-gui-hua-bd25/为例。

2、递归是一种自定向下不断取调用的过程（也就是递的过程），在达到边界时再自底向上地进行计算。而动态规划问题更像是直接从基本条件出发，舍弃了递归中的递，保留了递归中的归，直接自底向上地计算出最终木凑结果。

所以说，递归和动态规划从思维上是一回事，问题的核心在于确定子问题、确定子问题的边界。

3、DP和递归回溯本质是一回事，都是暴力搜索，所以在写DP时，只要能**保证把所有情况都覆盖上**，就能计算出最终答案。

## 一、基础

###1爬楼梯

1、爬楼梯 https://leetcode.cn/problems/climbing-stairs/description/

假设要爬到第i级台阶，那么共有两种情况

- 最后一步爬了1个台阶
- 最后一步怕了2个台阶

上述两种情况是独立的，所以从**0爬到i的方法数**为两种情况之和，即dfs(i-1) + dfs(i-2)

**抽象模型**：每次从1和2中选一个数，问有几种凑成i的方案。

2、最小花费爬楼梯 https://leetcode.cn/problems/min-cost-climbing-stairs/description/

同样是爬楼梯问题，爬到第i级台阶有两种情况，记爬到第i级台阶的最小花费为dfs(n)

- 从第i-1个台阶向上爬1个台阶
- 从第i-2个台阶向上爬2个台阶

所取得的最小花费是二者的最小值，即为   min(  dfs(n-1) + cost[n-1], dfs(n-2)+cost[n-2] )。定义边界时需要注意，dfs(0) =  dfs(1) = 0。

3、组合总和4 https://leetcode.cn/problems/combination-sum-iv

同样是爬楼梯问题，只是每次的选择数量变多了，有如下几种情况可以使得总和为n，记组合数为dfs(n)

- 从n-nums[0] 爬 nums[0]步
- 从n-nums[1] 爬 nums[1]步
- ...
- 从n-nums[n-1] 爬 nums[n-1]步

抽象模型：每次从nums中选一个数，有几种方案可以凑成target

==疑问==：为什么最终的dp[]数组会爆int？

4、构造好字符串的方案数：https://leetcode.cn/problems/count-ways-to-build-good-strings/

5、**增强版爬楼梯** https://leetcode.cn/problems/count-number-of-texts

6、二进制字符串https://leetcode.cn/problems/number-of-good-binary-strings/

-------

==爬楼梯模板==

对于爬楼梯类型的问题，都满足如下的关系
$$
dp[i]=\sum_{i=1}^{n}dp[i-nums[i]]
$$
参数：nums中存储可选的数字（一次能走几步），target表示目标值，dp数组表示到达指定下标的方案数。

```C++
int climbStairs(vector<int> &nums, int target) {
        vector<int> memo(target+1, -1);
        vector<unsigned> dp(target+1, 0);
        dp[0] = 1;
        for(int i = 1; i <= target; i++){
            for(int x : nums){
                if(i-x >= 0){
                    dp[i] += dp[i-x];
                }
            }
        }
        return dp[target];
}
```

==建议==

1、为了避免边界条件，建议直接把dp数组的范围直接拉到最大。比如1e5+10这种。

### 2打家劫舍

间隔性选择问题。

1. 打家劫舍https://leetcode.cn/problems/house-robber/description/

2. 删除并获取点数 https://leetcode.cn/problems/delete-and-earn/description/
3. 统计放置房子的方式数 https://leetcode.cn/problems/count-number-of-ways-to-place-houses/
4. 打家劫舍2 https://leetcode.cn/problems/house-robber-ii/



###3最大子数组和

1. 最大子数组和 https://leetcode.cn/problems/maximum-subarray  
2. 最大开销子串：https://leetcode.cn/problems/find-the-substring-with-maximum-cost/，思路和1一样，学到一个给a-z的字符串做初始化的小trick
3. 子数组和的绝对值的最大值：https://leetcode.cn/problems/maximum-absolute-sum-of-any-subarray，思路，子数组前缀和的最大最小值的差即为答案。
4. **k次串联后的最大数组和**：https://leetcode.cn/problems/k-concatenation-maximum-sum/，重点在于找后续的数学规律，如果数组和大于0，那肯定是越长越好。
5. **环形子数组最大和**：https://leetcode.cn/problems/maximum-sum-circular-subarray/。
   - 基本思路：分两种情况，一种情况求正常的不跨边界的最大子数组值，另一种情况求跨边界后的最大子数组值。
   - 疑问：灵神给的一个边界情况没怎么看懂。。
6. 拼接数组最大分数：https://leetcode.cn/problems/maximum-score-of-spliced-array/description/
   - 思路：换皮版的最大子数组和

## 二、背包

**核心思想**

概述：给定背包容量w和物品价值集合的情况下，怎么放背包能使得背包容量最大？

==01背包、完全背包==

01背包是顺序无关的



**常见的变式**

- 至多装capacity，求方案书、最大价值
- 恰好装capacity，求方案书、最大价值、最小价值
- 至少装capacity，求方案书、最小价值
- 从集合中取一些元素，使它们的总和与某个「定值」有关，那么可以考虑转换为背包问题. 



**遍历顺序问题**。针对如下的状态转移方程，如何确定遍历顺序？
$$
f[i][c] = min(~f[i-1][c],~~ f[i][c-w[i]]+v[i]~) \\
f[c]=min(~f[c],~~f[c-w[i]]+v[i])
$$


- 正向遍历
- 逆向遍历

==参考==

https://github.com/EndlessCheng/codeforces-go/blob/master/leetcode/SOLUTIONS.md



## 三、线性DP

1. 最长公共子序列 https://leetcode.cn/problems/longest-common-subsequence

   > 原问题：两个字符串的最长子序列长度
   >
   > 思路：字符串的子序列，本质就是根据每个字符**选或者不选**的情况，所组合出的一个序列。如果从字符串的末位字符开始做判断，那么可以把问题分解为以下几个子问题。s[0-i]  t[0-j]
   >
   > 1. 子序列带s[i] t[j]
   > 2. 子序列带s[i] 不带t[j]
   > 3. 子序列不带s[i]，带t[j]
   > 4. 子序列不带s[i]，不带t[j]
   >
   > 根据s[i]和t[j]是否相等，可以把上述子问题做进一步推演
   >
   > 1. s[i] = t[j]，那么原问题变成了   字符串s[0, i-1]和t[0, j-1]的最大子序列长度 + 1
   > 2. s[i] != t[j]，原问题变成  字符串s[0, i]和t[0, j-1]最大子序列长度、 字符串s[0, i-1]和t[0, j]最大子序列长度中的最大值
   >
   > 可以**把原问题拆分为若干个类似的、规模更小的子问题，可以用回溯（DP）解决。**
   >
   > 回溯：
   >
   > - 当前操作：考虑  s[i]和t[j]选或不选
   > - 子问题：s的前i个字母和t的前j个字母的LCS序列长度
   > - 下一个子问题，  i  j-1 LCS；  i-1 j LCS;  i-1 j-1 LCS

   $$
   dfs(i, j)=\left\{  
                \begin{array}{**lr**}  
                max\bigg(dfs(i-1,j-1)+1,dfs(i-1,j),dfs(i,j-1)\bigg), s[i]=t[j]&  \\  
                max\bigg(dfs(i-1,j-1),dfs(i-1,j),dfs(i,j-1)\bigg), s[i]!=t[j]& \\      
                \end{array}  
   \right.
   $$

   可以证明，上式可以简化为
   $$
   dfs(i, j)=\left\{  
                \begin{array}{**lr**}  
                dfs(i-1,j-1)+1 ,s[i]=t[j]&  \\  
                max\bigg(dfs(i-1,j),dfs(i,j-1)\bigg), s[i]!=t[j]& \\      
                \end{array}  
   \right.
   $$

   > 递推：一比一翻译

2. 编辑距离  https://leetcode.cn/problems/edit-distance/solutions/2133222/jiao-ni-yi-bu-bu-si-kao-dong-tai-gui-hua-uo5q/ 

   > 子问题：当前两个字符串的最小编辑距离
   >
   > - 末尾字符相同，dfs(i,j) = dfs(i-1, j-1)
   > - 末尾不同，则  dfs(i,j) = min (  dfs(i-1, j)  dfs(i, j-1)  dfs(i-1, j-1)  )  + 1
   > - dfs(i, j) 表示s的前i个字符到t的前j个字符的编辑距离

3. 最小asc删除和 https://leetcode.cn/problems/minimum-ascii-delete-sum-for-two-strings/

   > 子问题：当前字符串的最小删除asc和
   >
   > - 末尾字符相同： dfs(i, j) = dfs(i-1,j-1)
   > - 末尾字符不同：dfs(i,j) = min (  dfs(i-1, j)+ord[s1[i]]  dfs(i, j-1)+ord[s2[j]]  ) 
   > - dfs(i,j) 表示使得s1前i个字符串和s2前j个字符串相等所需要删除的字符的最小ascii和

4. 交错字符串：https://leetcode.cn/problems/interleaving-string

   > 原问题：s3字符串能否由s1和s2字符串交错组成。
   >
   > 分析：
   >
   > - 如果长度12串的长度不等于3，那么显然不可以。
   > - 观察发现，交错组成的字符串的尾元素一定是前两个子串中，某一个子串的尾元素，可以拆分出如下的子问题
   >
   > 1. 例如，如果s3尾元素s3[i+j]等于s1的元素s1[i]，那么问题可以进一步缩小为s3[i+j+1] 是否可由 s1[i+1]或s2[j]来表示
   > 2. 定义dfs(i, j)表示当前所处理的 s1的前i个字符和s2的前j个字符，是否能表示s3的前i+j个字符

   

5. 不同子序列：https://leetcode.cn/problems/distinct-subsequences

6. 不相交的线：https://leetcode.cn/problems/uncrossed-lines/description/

7. 最大点积 https://leetcode.cn/problems/max-dot-product-of-two-subsequences/solutions/2373972/zhu-bu-shen-ru-ji-yi-hua-sou-suo-dong-ta-7h09/   状态会找，但是边界问题没看明白。





##四、树状DP





##五、网格图DP

1. **不同路径**2：https://leetcode.cn/problems/unique-paths-ii/description/
2. https://leetcode.cn/problems/minimum-falling-path-sum/description/
3. **最大移动次数**https://leetcode.cn/problems/maximum-number-of-moves-in-a-grid/description/
4. https://leetcode.cn/problems/minimum-path-cost-in-a-grid/description/
5. https://leetcode.cn/problems/minimum-falling-path-sum-ii/
6. https://leetcode.cn/problems/maximum-non-negative-product-in-a-matrix/description/

都是自底向上的动态规划。





## tricks

1、Dp数组的边界问题

如果出现了负数下标，或者到了上下边界、右边界，就多加几行、多加几列。



2、边界确定



3、遍历顺序的选择






















