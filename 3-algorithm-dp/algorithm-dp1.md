## 分析方法

- 明确DP数组含义
- 找DP的递推式
- 对DP做正确的初始化
- 对DP做正确的遍历
- 动手推动DP



##简单DP

- 整数拆分：https://leetcode.cn/problems/integer-break/
- 上楼梯：https://leetcode.cn/problems/min-cost-climbing-stairs/description/

##**0-1背包**

模板

```c++
#include <iostream>
#include <algorithm>
using namespace std;
const int N = 1010;
int w[N], v[N];
int dp[N];

int main()
{
    // n物品数量，m背包容量
    // dp[i]，背包容量为i的情况下，最大的价值
    int n, m;
    cin >> n >> m;
    for(int i = 0; i < n; i++){
        cin >> w[i] >> v[i];
    }
    for(int i = 1; i <= n; i++)
        for(int j = m; j >= w[i]; j--)
            dp[j] = max(dp[j], dp[j-w[i]] + v[i]);
    
    cout << dp[m] << endl;
	return 0;
}
```

- 参考博客：https://blog.csdn.net/raelum/article/details/128996521

其中，01背包中内嵌的循环从大到小遍历，是为了保证每个物品只被添加一次。**外层遍历物品，内层遍历物品价值，从大到小遍历。**

```c++
    for(int i = 1; i <= n; i++)
        for(int j = m; j >= w[i]; j--)
            dp[j] = max(dp[j], dp[j-w[i]] + v[i]);
```

- 求和问题
  - https://leetcode.cn/problems/target-sum/description/
- 组合问题
  - https://leetcode.cn/problems/partition-equal-subset-sum/description/
  - https://leetcode.cn/problems/ones-and-zeroes/description/

##**完全背包**

完全背包的分析逻辑和01背包大同小异，区别只在于在完全背包问题中，一个物品可以被重复选择。这个差异体现在代码里就是内层嵌套的for可以从小到大遍历

```c++
 	for(int i = 1; i <= n; i++)
        for(int j = w[i]; j <= m; j++)
            dp[j] = max(dp[j], dp[j-w[i]] + v[i]);
```

- 换零钱
  - https://leetcode.cn/problems/coin-change-ii/description/
  - https://leetcode.cn/problems/coin-change/description/



##背包问题总结

对于应用题，首要要分清楚谁是背包、谁是物品，如果我们的目标和物品的顺序高度相关，那就先遍历背包容量；如果对顺序要求不高，那就先遍历物品。

### 递推公式

1、最多装多少，能否装满

dp含义：容量为j时，可以装下的最大价值
$$
dp^{(i)}[j] = max\bigg (dp^{(i-1)}[j],~~~dp^{(i)}\Big[j-nums[i]\Big] + nums[i]\bigg )
$$
2、装满背包有多少种方式

dp含义：容量为j时，装满背包的方式数量
$$
dp^{(i)}[j] = dp^{(i-1)}[j]+dp^{(i)}\Big[j-nums[i]\Big]
$$
3、背包装满后的最大价值
$$
dp^{(i)}[j] = max\bigg (dp^{(i-1)}[j],~~~dp^{(i)}\Big[j-value[i]\Big] + value[i]\bigg )
$$

4、装满背包所有物品的最小个数

dp含义：容量为j的前提下，装满背包的物品数量
$$
dp^{(i)}[j]=min\bigg( dp^{(i)}\Big[j-coins[i]\Big]+1, ~~dp^{(i-1)}[j]\bigg)
$$



### 遍历顺序

==遍历顺序问题==

完全背包dp中for循环的嵌套顺序问题，需要根据题目**是否强调物品顺序**确定。

1. 强调排列性的数据，外层循环对价值做遍历
2. 不强调排列性，外层对物品做遍历

- 组合总和：https://leetcode.cn/problems/combination-sum-iv/description/

- 零钱拆分1：https://leetcode.cn/problems/coin-change/description/

- 零钱拆分2：https://leetcode.cn/problems/coin-change-ii/
  $$
  DP^{i}(j)=DP^{i-1}(j)~~~~~ j <= coins[i] \\DP^{i}(j)=DP^{i-1}(j) + DP^{i}(j-coins[i])~~~~~ j >= coins[i]
  $$



##**线性DP**



##**树形DP**

打家劫舍3：https://leetcode.cn/problems/house-robber-iii/description/





```c++
#include <iostream>
#include <vector>

using namespace std;

const int MAX_N = 5010;
int space[MAX_N];
int value[MAX_N];

int main()
{
    int M, N;
    cin >> M >> N;
    for(int i = 0; i < M; i++){
        cin >> space[i];
        cout << space[i] << " ";
    }
    puts("");
    for(int i = 0; i < M ; i++){
        cin >> value[i];
        cout << value[i] << " ";
    }
    puts("");
        
    cout << max_value() << endl;
    return 0;
}
```

