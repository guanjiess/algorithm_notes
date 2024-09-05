区间问题都可以考虑用双指针。

## 双指针

双指针法，通过一个快指针和一个慢指针，用一个for循环完成两个for循环的操作。

**滑动窗口的精妙之处在于根据当前子序列和大小的情况，不断调节子序列的起始和结束位置。从而将O(n^2)的暴力解法降为O(n)。** 

==应用场景==

- 单调性：数据的单调递增递减，或者条件上的满足、不满足。



###相向双指针-有序

在有序的条件下，使用双指针能够把暴力做法的O(n2)复杂度优化至O(n)，因为每次对比的都是最大、最小的极端情况，利用O(1)的时间复杂度，获取了O(n)级别的信息量。

- max + min > target，意味着左指针min右侧所有数字都大于 target，信息量是O(n)级别的。
- 归根结底，是利用了数组本身的有序性。



1. 两数之和  https://leetcode.cn/problems/two-sum-ii-input-array-is-sorted/solution/san-shu-zhi-he-bu-hui-xie-xiang-xiang-sh-6wbq/ 

2. 三数之和： https://leetcode.cn/problems/3sum/solution/shuang-zhi-zhen-xiang-bu-ming-bai-yi-ge-pno55/  

   > 去重逻辑：当前的数和上一个数相同，那就跳到下一个数。

3. 四数之和：https://leetcode.cn/problems/4sum/ 

4. 最接近的三数之和 https://leetcode.cn/problems/3sum-closest/ 

5. 有效三角形的个数 https://leetcode.cn/problems/valid-triangle-number

   > 思路：其实和三数之和很接近，最终是求  a + b < c，也是三个数之间和的关系。可以枚举最大的边c，然后从剩余的边中按照  极大-->极小   极小-->极大  的顺序遍历。

6. 盛最多水的容器： https://leetcode.cn/problems/container-with-most-water/solution/by-endlesscheng-f0xz/ 

   > 思路：雨水体积为  V = (R - L) * min(HL, HR)。左右板中，短板决定了体积，只有拉高短板才有可能提高整体容量。

7. 接雨水： https://leetcode.cn/problems/trapping-rain-water/solution/zuo-liao-nbian-huan-bu-hui-yi-ge-shi-pin-ukwm/ 

   > 法一：前后缀最大值。
   >
   > 第i个格子处的雨水体积为：  min(pre_max, suf_max) - height(i)。只需要求出前后缀最大的数组即可。
   >
   > 法二：相向双指针。其实就是对法一的优化。



##定长滑窗

原则：窗口内的元素，右边进、左边出。

实现细节

1. 假定定长窗口长度为n，每次循环操作前，左右指针之间维护一个长为k-1的窗口
3. 进循环，从窗口右端进入一个数据
4. 对窗口内的数据做操作。
5. 左端窗口收缩、删除相应数据。

```python
l = 0
for r in range(k-1, n):
    ### 1.具体执行逻辑，判断窗内元素性质
    
    ### 2.收缩左端点，删除相应数据等
    l += 1
```

**python简洁写法：同时送入两个数组，一个进、一个出，省区了判断边界**

```python
# https://leetcode.cn/problems/minimum-swaps-to-group-all-1s-together-ii/
class Solution:
    def minSwaps(self, nums: List[int]) -> int:
        
        cnt1, max_cnt = sum(nums[:k]), 0
        for _in, _out in zip(nums[k:], nums):
            cnt1 += _in - _out
            max_cnt = max(cnt1, max_cnt)
        return ones - max_cnt
```





###练习

1. 爱生气的书店老板：https://leetcode.cn/problems/grumpy-bookstore-owner/
2.  [2841. 几乎唯一子数组的最大和](https://leetcode.cn/problems/maximum-sum-of-almost-unique-subarray/) 
3.  [2461. 长度为 K 子数组中的最大和](https://leetcode.cn/problems/maximum-sum-of-distinct-subarrays-with-length-k/) 
4.  [1423. 可获得的最大点数](https://leetcode.cn/problems/maximum-points-you-can-obtain-from-cards/) 
5.  [2134. 最少交换次数来组合所有的 1 II](https://leetcode.cn/problems/minimum-swaps-to-group-all-1s-together-ii/) 
6. [2653. 滑动子数组的美丽值](https://leetcode.cn/problems/sliding-subarray-beauty/)   ==不熟==
7. [567. 字符串的排列](https://leetcode.cn/problems/permutation-in-string/)
8. [483 字母异位词](https://leetcode.cn/problems/find-all-anagrams-in-a-string/)
9.  [2156. 查找给定哈希值的子串](https://leetcode.cn/problems/find-substring-with-given-hash-value/)   涉及到模运算，暂时跳过。



###易错&tips

1. 环形数组可以扩容两倍，形成一个链条。如[2134. 最少交换次数来组合所有的 1 II](https://leetcode.cn/problems/minimum-swaps-to-group-all-1s-together-ii/) 
2. 枚举左右端点时，注意数据范围，不要越界、不要漏数。如[1423. 可获得的最大点数](https://leetcode.cn/problems/maximum-points-you-can-obtain-from-cards/) 



## 不定长滑窗

原则：枚举右端点、收缩左端点。想象一下毛毛虫。

```python
l = 0
for r in range(n):
    ### 1.对右端点执行相应操作，判断窗内元素性质
    
    ### 2.收缩左端点（满足条件->不满足 or 不满足条件->满足），删除相应数据等
    
    ### 3.计入答案
    
```



### 最长、最大

1. 无重复字符的最长子串：https://leetcode.cn/problems/longest-substring-without-repeating-characters/solution/xia-biao-zong-suan-cuo-qing-kan-zhe-by-e-iaks/ 

   ```python
   class Solution:
       def lengthOfLongestSubstring(self, s: str) -> int:
           n = len(s)
           l, r = 0, 0
           ans = 0
           cnt = Counter() # char : int
           # 用哈希表记录窗口中元素出现次数，右进左出
           # 从右端进元素，如果出现重复，必然是右端进入的元素
           for r in range(n):
               cnt[s[r]] += 1
               while cnt[s[r]] > 1:
                   cnt[s[l]] -= 1
                   l += 1
               ans = max(ans, r-l+1)
           return ans
   ```

   - 时间复杂度：O(n)
   - 空间复杂度：O(128)  ascii字符集有128个字符

2. [2730. 找到最长的半重复子字符串](https://leetcode.cn/problems/find-the-longest-semi-repetitive-substring/) 

3. [1695. 删除子数组的最大得分](https://leetcode.cn/problems/maximum-erasure-value/) 

4. [2958. 最多 K 个重复元素的最长子数组](https://leetcode.cn/problems/length-of-longest-subarray-with-at-most-k-frequency/)   注： 2-4三道题几乎一样。

5. [2024. 考试的最大困扰度](https://leetcode.cn/problems/maximize-the-confusion-of-an-exam/)   稍微饶了点弯

6. [1004. 最大连续1的个数 III](https://leetcode.cn/problems/max-consecutive-ones-iii/) 

7. https://leetcode.cn/problems/find-the-longest-equal-subarray/  ==非常优雅==

8. [1438. 绝对差不超过限制的最长连续子数组](https://leetcode.cn/problems/longest-continuous-subarray-with-absolute-diff-less-than-or-equal-to-limit/)   ==滑窗+有序数据结构==

   - C++: `set`    `map`  `priority_queue等`
   - python:  `sortedcontainers`
   - 或者维护两个单调队列，一个头部放最大、一个放最小

9. [2401. 最长优雅子数组](https://leetcode.cn/problems/longest-nice-subarray/)   ==滑窗 + 位运算==

10.  [将 x 减到 0 的最小操作数](https://leetcode.cn/problems/minimum-operations-to-reduce-x-to-zero/)   ==正难则反==

11. [1838. 最高频元素的频数](https://leetcode.cn/problems/frequency-of-the-most-frequent-element/) ==优雅==

12. [2516. 每种字符至少取 K 个](https://leetcode.cn/problems/take-k-of-each-character-from-left-and-right/)  正难则反

13.  [2106. 摘水果](https://leetcode.cn/problems/maximum-fruits-harvested-after-at-most-k-steps/)   ==二分+滑窗==  暂时碰不了这个难度



### 最短、最小

1. 长度最小的子数组 https://leetcode.cn/problems/minimum-size-subarray-sum/solution/biao-ti-xia-biao-zong-suan-cuo-qing-kan-k81nh/ 

   ```python
   class Solution:
       def minSubArrayLen(self, target: int, nums: List[int]) -> int:
           ans = inf  #便于取min 
           s, l = 0, 0
           for r in range(len(nums)):
               s += nums[r]
               #判断是否可收缩
               while s - nums[l] >= target:
                   s -= nums[l]
                   l += 1
               #必要判断，无法根据 s-nums[l] < target得出s >= target
               if s >= target: 
                   ans = min(ans, r-l+1)
           return ans if ans != inf else 0
   ```

   - 时间复杂度：O(n)，左右指针至多加到 n。
   - 空间复杂度：O(1)，只用了几个额外的变量。
   - 满足单调性，才可以适用双指针。不断收缩左端点，从满足条件直到不满足为止。

- [1234. 替换子串得到平衡字符串](https://leetcode.cn/problems/replace-the-substring-for-balanced-string/) 1878

- [1574. 删除最短的子数组使剩余数组有序](https://leetcode.cn/problems/shortest-subarray-to-be-removed-to-make-array-sorted/) 1932

  > 核心目的把左右两端的递增子数组拼起来。

- [76. 最小覆盖子串](https://leetcode.cn/problems/minimum-window-substring/)

最短相比最长类型的题单，更灵活一些。



### 子数组个数

1. 乘积小于 K 的子数组 https://leetcode.cn/problems/subarray-product-less-than-k/solution/xia-biao-zong-suan-cuo-qing-kan-zhe-by-e-jebq/ 

   ```python
   class Solution:
       def numSubarrayProductLessThanK(self, nums: List[int], k: int) -> int:
           if k <= 1:
               return 0
           ans = 0
           prod, l = 1, 0
           for r in range(len(nums)):
               # 枚举右端点
               prod *= nums[r]
   
               # 如果不满足条件，收缩左端点到满足为止
               while prod >= k:
                   prod /= nums[l]
                   l += 1
               print(l, r)
               # 满足条件，计入答案
               ans += r - l + 1
           return ans
   ```

   

2. 自定义：在[1, n]的范围内，有多少个子区间的和为n？--->连续子数组和为target



问题

- 为什么滑动窗口的代码里也是套了两层循环，但是时间复杂度依然是O(n)？因为滑动窗口里的每个数据都只有两次操作，进一次、出一次。



## 多指针





##快慢指针法

双指针法的核心思想是两个指针分别从两端开始,向中间移动,每次移动一个指针,每次移动时都检查两个指针所指向的元素是否满足某种条件,如果满足条件则进行相应的操作,否则继续移动指针。

双指针法的时间复杂度为 O(n),空间复杂度为 O(1)。

