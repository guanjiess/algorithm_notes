## 二分答案-最小

1. 二分就是猜答案。

2. H指数，不是很懂：https://leetcode.cn/problems/h-index-ii/description/

3. 最小化最大值：https://leetcode.cn/problems/minimize-maximum-of-array/

4. 搜索旋转排序数组：https://leetcode.cn/problems/find-minimum-in-rotated-sorted-arra

5.  [1283. 使结果不超过阈值的最小除数](https://leetcode.cn/problems/find-the-smallest-divisor-given-a-threshold/) 

   > 1. 在使用二分法求答案时，初始的L和R的区间一定要覆盖所有可能情况。
   > *  以左、右开区间为例，(l,r) 应该囊括所有的可能区间，如果该区间是 [0,5]，那么开区间形式写法就是(1, 6)
   > 2. 该题的循环不变量是。l及其左侧，不满足条件；r及其右侧，满足条件。根据mid的形式来更新循环不变量的区间。
   > * check(r) <= threshold
   > * check(l) > threshold

6. [1870. 准时到达的列车最小时速](https://leetcode.cn/problems/minimum-speed-to-arrive-on-time/) 

7. [1011. 在 D 天内送达包裹的能力](https://leetcode.cn/problems/capacity-to-ship-packages-within-d-days/) 

   ```C++
   class Solution {
   public:
       int shipWithinDays(vector<int>& weights, int days) {
           auto check = [&](int x)->bool{ //载荷 load = x
               int cnt = 0, total_days = 0;
               // 统计所有包裹需要的总运输时间
               for(int i = 0; i < weights.size(); i++){
                   if (weights[i] > x) return false;  // 特判，不可能成立的情况
                   if(cnt + weights[i] <= x){             // 当天可继续运输
                       cnt += weights[i];
                   } else{
                       total_days += 1;
                       cnt = weights[i];                  // 第二天再运
                   }
               }
               total_days += cnt > 0;                 // 最后一天有剩余的，单独运输
               return total_days <= days;
           };
           int l = 0, r = accumulate(weights.begin(), weights.end(), 0) + 1;
           while (l+1 < r){
               int mid = l + (r-l)/2;
               if(check(mid)){     // 循环不变量：check(r) = true; check(l) = false
                   r = mid;
               } else{
                   l = mid;
               }
           }
           return r;
       }
   };
   ```

8. [475. 供暖器](https://leetcode.cn/problems/heaters/)  ==值得思考==，但是怎么用二分答案写出来？

9. [2594. 修车的最少时间](https://leetcode.cn/problems/minimum-time-to-repair-cars/)   比较套路，难在怎么往二分的方向去想。

10. [1482. 制作 m 束花所需的最少天数](https://leetcode.cn/problems/minimum-number-of-days-to-make-m-bouquets/) 



## 二分答案-最大

1. [275. H 指数 II](https://leetcode.cn/problems/h-index-ii/)

2. [2226. 每个小孩最多能分到多少糖果](https://leetcode.cn/problems/maximum-candies-allocated-to-k-children/)  

3. [1898. 可移除字符的最大数目](https://leetcode.cn/problems/maximum-number-of-removable-characters/)  ==学了不少字符处理的trick==

4. [1802. 有界数组中指定下标处的最大值](https://leetcode.cn/problems/maximum-value-at-a-given-index-in-a-bounded-array/)   ==数学 + 贪心 + 二分答案==

   典型：求最大值时要返回left，因为最后一个满足 `check(mid) = true`的结果保存在left.

   ```python
   class Solution:
       def maxValue(self, n: int, index: int, maxSum: int) -> int:
           L = index + 1
           R = n - index
           def check(mid):
               ans = 0
               if L >= mid:  # 会减到 1
                   ans += (mid + 1) * mid // 2 + L - mid
               else:  # 减不到1
                   ans += (mid + mid - L + 1) * L // 2 
               if R >= mid:
                   ans += (mid + 1) * mid // 2 + R - mid
               else:
                   ans += (mid + mid - R + 1) * R // 2
               ans -= mid  #删去重复的mid
               return ans
   
           l, r = 0, 1e9 + 1
           while l + 1 < r:
               mid = (l + r) // 2
               if check(mid) <= maxSum:   # check(r) > maxSum
                   l = mid
               else:
                   r = mid
           return round(l)  # 最后一个check(mid) <= maxSum的结果保存在 l中
   ```

5. [1642. 可以到达的最远建筑](https://leetcode.cn/problems/furthest-building-you-can-reach/)  ==贪心+二分==

   主打一个匹配性。梯子适合用在高度差大的、砖块适合用在高度差小的。

   计算差分数组可以降低部分处理难度。

   精妙。

6. [2861. 最大合金数](https://leetcode.cn/problems/maximum-number-of-alloys/)   小tricks值得注意

   ```python
   def check(num, composition, stock, budget):
       money = 0    
       for s, comp, c in zip(stock, composition, cost):
            if s < num * comp:
                 money += (comp * num - s) * c
                 f money > budget:
                     return False
        return True
   ```

   



## 最小化最大

1. [410. 分割数组的最大值](https://leetcode.cn/problems/split-array-largest-sum/)

> ​	 贪心思维：做check验证时，尽可能分割若干个子串使其和为 mx
>
> ​    1.怎么保证的最大值 最小？
>
> ​      二分查找的性质。循环不变量是  check(r) = true  check(l) = false，当检测到check(mid)=true时
>
> ​      会将r向内部收缩，顾最后一个满足条件的r就是最小的。
>
> ​    2.怎么保证可以分割为k个子段？
>
> ​      check(mx)相当于锁死了子段和的上限。（二分枚举上限）如果本身就能分割成k段，那OK；如果不能分割成k段 和为 mx的，那就把其中      某几段拆开，凑够k段。

2. [2064. 分配给商店的最多商品的最小值](https://leetcode.cn/problems/minimized-maximum-of-products-distributed-to-any-store/)   判断商家越界

3. [1760. 袋子里最少数目的球](https://leetcode.cn/problems/minimum-limit-of-balls-in-a-bag/)  判断操作次数

4. [1631. 最小体力消耗路径](https://leetcode.cn/problems/path-with-mini mum-effort/)  ==二分+网格图BFS==

5. [2439. 最小化数组中的最大值](https://leetcode.cn/problems/minimize-maximum-of-array/)   ==二分+贪心==

   > 猜测一个上界 extra，即要求所有操作后的元素值不超过limit。题目要求中，数据只能从右到左流动，`extra = nums[i] - limit > 0` ，可以把多余的部分添加到 `nums[i-1]` 上。最后判断 nums[0] 是否大于 limit



思路：枚举上界，判定是否存在突破上界的可能。

一个普遍的套路是，把待查找的数据尽可能摊平，如果摊平之后依然存在一个大于上限的值，说明改条件不成立。



## 最大化最小

1.  [2517. 礼盒的最大甜蜜度](https://leetcode.cn/problems/maximum-tastiness-of-candy-basket/)  
2.  [1552. 两球之间的磁力](https://leetcode.cn/problems/magnetic-force-between-two-balls/) 



## 其他关键点

###0. 单调

使用二分答案的前提条件是单调性。单调性指的是

1. 数据上的单调，在区间内满足递增或者递减。
2. 性质上的单调，在区间内恒满足于某个条件。

### 1. 区间

1. 区间选择要包括所有可能情况，范围上做到不漏。
   - 左边界选一定不成立的、选下界以下
   - 右边界选一定成立的、选上界以上
2. 如果对数组的下标做二分搜索，需要注意不要让下标越界。如
   - [1385. 两个数组间的距离值](https://leetcode.cn/problems/find-the-distance-value-between-two-arrays/)

###2.循环不变量（最大、最小）

我的理解是，如果在某个区间内，一些条件恒成立，那么这个条件就是循环不变量。

1. 求最小：`check(mid) == true`时更新  `right = mid`，反之更新 `left = mid`。最终返回 right
2. 求最大：`check(mid) == true`时更新  `left = mid`，反之更新 `right = mid`。最终返回  left