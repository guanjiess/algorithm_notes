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



## 参考

[差分数组](https://leetcode.cn/circle/discuss/FfMCgb/)