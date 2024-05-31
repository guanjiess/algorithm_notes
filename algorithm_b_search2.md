## 二分答案

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

6.  [1870. 准时到达的列车最小时速](https://leetcode.cn/problems/minimum-speed-to-arrive-on-time/) 





## 循环不变量



