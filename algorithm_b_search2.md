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
4. [1802. 有界数组中指定下标处的最大值](https://leetcode.cn/problems/maximum-value-at-a-given-index-in-a-bounded-array/)   dfs怎么搜索？





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