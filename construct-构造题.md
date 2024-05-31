##典型构造题

1. 重构字符串：https://leetcode.cn/problems/reorganize-string/description/











##数学相关

1. 最大工作周数：https://leetcode.cn/problems/maximum-number-of-weeks-for-which-you-can-work
2. 最小开销：https://leetcode.cn/problems/minimum-cost-to-equalize-array/description/
3. 缺失观测数据：https://leetcode.cn/problems/find-missing-observations/





## 分类讨论

1. https://leetcode.cn/problems/find-longest-special-substring-that-occurs-thrice-ii

```python
class Solution:
    def maximumLength(self, s: str) -> int:
        groups = defaultdict(list)
        cnt = 0
        for i,c in enumerate(s):
            cnt += 1 # 统计连续子串长度
            if i+1 == len(s) or c != s[i+1]: #当前字符和后续字符不同，或已到达子串末尾
                groups[c].append(cnt)
                cnt = 0
        ans = -1
        for k,idx in groups.items():
            """
            case1. 从最长子串中选出 长为a[0]-2子串
            case2. 从最长、次长子串 a[0] a[1]中选三个子串
                ---> a[0] > a[1], 答案为 a[1]
                ---> a[0] = a[1], 答案为 a[0]-1
            case3. 
            """
            # 
            idx.sort(reverse=True)
            idx.extend([0,0])  # trick，避免边界条件处理
            print(idx)
            case1 = idx[0] - 2
            case2 = idx[1] if idx[0] > idx[1] else idx[0]-1
            case3 = idx[2]
            ans = max(ans, case1, case2, case3)
        return ans if ans != 0 else -1
```



