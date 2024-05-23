##位运算基本操作

1. 与   `&`
2. 或   `|`  ： 相当于加
3. 非   `~`
4. 异或 `^`  ： 相当于减





## 与或（AND/OR）





##异或（XOR）

1.  [2401. 最长优雅子数组](https://leetcode.cn/problems/longest-nice-subarray/)  

   ```python
   class Solution:
       def longestNiceSubarray(self, nums: List[int]) -> int:
           ans = 0
           # 法1： 暴力枚举
           # for i, or_ in enumerate(nums):
           #     j = i - 1
           #     while j >= 0 and (nums[j] & or_ == 0):
           #         or_ |= nums[j]
           #         j -= 1
           #     ans = max(ans, i-j)
   
           # return ans
   
           # 法2. 双指针
           l, or_ = 0, 0
           for r in range(len(nums)):
               while or_ & nums[r]:
                   or_ ^= nums[l]   # 这里的异或相当于把 nums[l]的影响从or_中消除
                   l += 1
               or_ |= nums[r]
               sz = r - l + 1
               if sz > ans:
                   ans = sz
           return ans
   ```

2. 





## 面试

1. 对于整数n，是否存在一个数k，使得2^k = n？

   ```C++
   int cnt = 0;
   while(n){
       cnt ++;
       n >> 1;
   }
   return cnt > 0;
   ```

   ```C++
   return n & (n-1)
   ```

   

2. 















## bulitin

###python





###C++

-  __builtin_popcount，统计整数二进制表示中1的数量

  ```C++
   __builtin_popcount(lower & upper & ~invalid)
  ```

- 