##四、树状DP





##五、网格图DP

1. **不同路径**2：https://leetcode.cn/problems/unique-paths-ii/description/
2. https://leetcode.cn/problems/minimum-falling-path-sum/description/
3. **最大移动次数**https://leetcode.cn/problems/maximum-number-of-moves-in-a-grid/description/
4. https://leetcode.cn/problems/minimum-path-cost-in-a-grid/description/
5. https://leetcode.cn/problems/minimum-falling-path-sum-ii/
6. https://leetcode.cn/problems/maximum-non-negative-product-in-a-matrix/description/

都是自底向上的动态规划。



## 六、状态机DP

###总结



###买卖股票系列 

1. 股票1 https://leetcode.cn/problems/best-time-to-buy-and-sell-stock

2. 股票2 https://leetcode.cn/problems/best-time-to-buy-and-sell-stock-ii 

   ```python
   def maxProfit(self, prices: List[int]) -> int:
       n = len(prices)
       @cache
       def dfs(i, hold):
           if i < 0:    #适用性更广的边界
               return -inf if hold else 0
           if hold:  #持有股票
               return max(dfs(i-1,True), dfs(i-1, False) - prices[i])
           return max(dfs(i-1, False), dfs(i-1, True) + prices[i]) #未持有
   
       return dfs(n-1, False)
   
   def maxProfit(self, prices: List[int]) -> int:
       n = len(prices)
       f = [[0]*2 for _ in range(n+1)]
       f[0][1] = -inf
       for i, p in enumerate(prices):
           f[i][0] = max(f[i-1][0], f[i-1][1] + prices[i])
           f[i][1] = max(f[i-1][1], f[i-1][0] - prices[i])
       return f[n][0]
           
   ```

   

3. 股票3  https://leetcode.cn/problems/best-time-to-buy-and-sell-stock-iii/ 

4. 股票4   https://leetcode.cn/problems/best-time-to-buy-and-sell-stock-iv **不熟**

5. 股票含冷冻期  https://leetcode.cn/problems/best-time-to-buy-and-sell-stock-with-cooldown/ **不熟**

   ```python
   class Solution:
       def maxProfit(self, prices: List[int]) -> int:
           n = len(prices)
           @cache
           def dfs(i: int, hold: bool) -> int:
               if i < 0:
                   return -inf if hold else 0
               if hold:
                   # 问题：第i-2天未持有股票，买入之后，第i-1天不就是 dfs(i-1, true)?
                   return max(dfs(i - 1, True), dfs(i - 2, False) - prices[i])
               return max(dfs(i - 1, False), dfs(i - 1, True) + prices[i])
           return dfs(n - 1, False)
   ```

   

   > 某一天持有股票，有可能前一天已经持有股票，也可能是从**两天前**过来。

6. 股票含手续费  https://leetcode.cn/problems/best-time-to-buy-and-sell-stock-with-transaction-fee/ 

7. 子序列交替和  https://leetcode.cn/problems/maximum-alternating-subsequence-sum/ 



##七、区间DP

1. 最长回文子序列 https://leetcode.cn/problems/longest-palindromic-subsequence/solution/shi-pin-jiao-ni-yi-bu-bu-si-kao-dong-tai-kgkg/ 

2. 多边形三角剖分的最低得分 https://leetcode.cn/problems/minimum-score-triangulation-of-polygon/solution/shi-pin-jiao-ni-yi-bu-bu-si-kao-dong-tai-aty6/ 

   注意遍历顺序问题。

3. 









