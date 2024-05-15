区间问题都可以考虑用双指针。

## 双指针

双指针法，通过一个快指针和一个慢指针，用一个for循环完成两个for循环的操作。

**滑动窗口的精妙之处在于根据当前子序列和大小的情况，不断调节子序列的起始和结束位置。从而将O(n^2)的暴力解法降为O(n)。** 





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

1. 假定定长窗口长度为n，每次循环操作前，左右指针之间维护一个长为n-1的窗口
3. 进循环，从窗口右端进入一个数据
4. 对窗口内的数据做操作。
5. 左端窗口收缩、删除相应数据。

```python
l = 0
for r in  range(n):
    ### 具体执行逻辑
    
    ###收缩左端点，删除相应数据等
    l += 1
```



2. 字母异位词：https://leetcode.cn/problems/find-all-anagrams-in-a-string/



## 不定长滑窗

原则：枚举右端点、收缩左端点。想象一下毛毛虫。

### 最长、最大

1. 无重复字符的最长子串：https://leetcode.cn/problems/longest-substring-without-repeating-characters/solution/xia-biao-zong-suan-cuo-qing-kan-zhe-by-e-iaks/ 

### 最短、最小

1. 长度最小的子数组 https://leetcode.cn/problems/minimum-size-subarray-sum/solution/biao-ti-xia-biao-zong-suan-cuo-qing-kan-k81nh/ 

### 子数组个数

1. 乘积小于 K 的子数组 https://leetcode.cn/problems/subarray-product-less-than-k/solution/xia-biao-zong-suan-cuo-qing-kan-zhe-by-e-jebq/ 
2. 自定义：在[1, n]的范围内，有多少个子区间的和为n？--->连续子数组和为target





问题

- 为什么滑动窗口的代码里也是套了两层循环，但是时间复杂度依然是O(n)？因为滑动窗口里的每个数据都只有两次操作，进一次、出一次。



快慢窗口



##快慢指针法

双指针法的核心思想是两个指针分别从两端开始,向中间移动,每次移动一个指针,每次移动时都检查两个指针所指向的元素是否满足某种条件,如果满足条件则进行相应的操作,否则继续移动指针。

双指针法的时间复杂度为 O(n),空间复杂度为 O(1)。

