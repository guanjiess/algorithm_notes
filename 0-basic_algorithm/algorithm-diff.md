## 差分数组

对于数组$a = [1,3,3,5,8]$，其差分数组为$d = [1, 2, 0, 2, 3]$。显然，从左到右累加$d$中的元素，可以**还原数组a**。
$$
d[i]=\left\{
\begin{array}{}
	a[0], 		&i=0\\
	a[i]-a[i-1],&i\ge1
\end{array}

\right.
$$

###性质1

从左到右累加d中元素，可以还原原数组

###性质2

对**数组a的区间**做同一的加减操作，和**差分数组的单点操作**等价。
$$
\left\{
\begin{array}{}
	a[i] =a[i]+ x, 		&i\in[i,j]\\
	d[i]=d[i]+x~~~~~d[j+1]=d[j+1]-x&i\ge1
\end{array}

\right.
$$

### 模板

python

```python
# 你有一个长为 n 的数组 a，一开始所有元素均为 0。
# 给定一些区间操作，其中 queries[i] = [left, right, x]，
# 你需要把子数组 a[left], a[left+1], ... a[right] 都加上 x。
# 返回所有操作执行完后的数组 a。
def solve(n: int, queries: List[List[int]]) -> List[int]:
    diff = [0] * n  # 差分数组
    for left, right, x in queries:
        diff[left] += x
        if right + 1 < n:
            diff[right + 1] -= x
    for i in range(1, n):
        diff[i] += diff[i - 1]  # 直接在差分数组上复原数组 a
    return diff
```



C++

```C++
// 你有一个长为 n 的数组 a，一开始所有元素均为 0。
// 给定一些区间操作，其中 queries[i] = [left, right, x]，
// 你需要把子数组 a[left], a[left+1], ... a[right] 都加上 x。
// 返回所有操作执行完后的数组 a。
vector<int> solve(int n, vector<vector<int>> queries) {
    vector<int> diff(n); // 差分数组
    for (auto &q: queries) {
        int left = q[0], right = q[1], x = q[2];
        diff[left] += x;
        if (right + 1 < n) {
            diff[right + 1] -= x;
        }
    }
    for (int i = 1; i < n; i++) {
        diff[i] += diff[i - 1]; // 直接在差分数组上复原数组 a
    }
    return diff;
}
```

模板题

1. 拼车 https://leetcode.cn/problems/car-pooling/description
2. 航班预定统计 https://leetcode.cn/problems/corporate-flight-bookings/description
3. [56. 合并区间](https://leetcode.cn/problems/merge-intervals/)
4. [57. 插入区间](https://leetcode.cn/problems/insert-interval/)  ==差分反倒显得繁琐，但是涉及的lambda排序值得去看==
5. 字母移位 https://leetcode.cn/problems/shifting-letters-ii/description
6. 区间划分为最小组数 https://leetcode.cn/problems/divide-intervals-into-minimum-number-of-groups
7. 分割数组为连续子序列 https://leetcode.cn/problems/split-array-into-consecutive-subsequences/description/
8. [732. 我的日程安排表 III](https://leetcode.cn/problems/my-calendar-iii/)
9. [732. 我的日程安排表 II](https://leetcode.cn/problems/my-calendar-ii/) ==在我看来是差分的范例==
10.  [2406. 将区间分为最少组数](https://leetcode.cn/problems/divide-intervals-into-minimum-number-of-groups/) 
11. [2381. 字母移位 II](https://leetcode.cn/problems/shifting-letters-ii/)   两道经典。





## 应用场景

- 上下车模型

- 区间的分配

- 日程安排、会议安排等

  

  相关问题出现比较多。

## 参考

[差分数组](https://leetcode.cn/circle/discuss/FfMCgb/)