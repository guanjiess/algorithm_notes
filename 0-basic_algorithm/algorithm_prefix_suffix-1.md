##一维前缀和

描述：给定一个数组，求数组指定范围内的和

<img src=".\algorithm_prefix.assets\1711589770286.png" alt="1711589770286" style="zoom: 50%;" />

```C++
#include <algorithm>
using namespace std;
const int N = 1e6 + 10;
int a[N], s[N], n, m;

int main(){
    cin >> n >> m;
    for(int i = 1; i <= n; i++){
        cin >> a[i];
    }
    s[0] = 0;
    for(int i = 0; i < n; i++){
        s[i+1] = s[i] + a[i];
    }
    while(m--){
        int l;
        int r;
        cin >> l >> r;
        printf("%d\n", s[r+1] - s[l]);
    }
    return 0;
}
```

### 前缀+哈希

- [560. 和为 K 的子数组](https://leetcode.cn/problems/subarray-sum-equals-k/)
- [1524. 和为奇数的子数组数目](https://leetcode.cn/problems/number-of-sub-arrays-with-odd-sum/) 
-  [974. 和可被 K 整除的子数组](https://leetcode.cn/problems/subarray-sums-divisible-by-k/)  
-  [1590. 使数组和能被 P 整除](https://leetcode.cn/problems/make-sum-divisible-by-p/)   ==tough==，需要理解 同余定理、负数取模，多看几遍吧，离摸透还差的远。
-  [523. 连续的子数组和](https://leetcode.cn/problems/continuous-subarray-sum/)
-  [437. 路径总和 III](https://leetcode.cn/problems/path-sum-iii/)  这两题都要注意边界问题。 前缀和  +  二叉树递归
-  [2588. 统计美丽子数组数目](https://leetcode.cn/problems/count-the-number-of-beautiful-subarrays/) 
- [525. 连续数组](https://leetcode.cn/problems/contiguous-array/)  和上一题思路很像。一个是用异或累计效果、一个是用一点点编码累计效果。
- https://leetcode.cn/problems/binary-subarrays-with-sum/description/



### 前缀距离和

- [1685. 有序数组中差绝对值之和](https://leetcode.cn/problems/sum-of-absolute-differences-in-a-sorted-array/) 
- [2615. 等值距离和](https://leetcode.cn/problems/sum-of-distances/) ==两道题如出一辙==



## 二维前缀和

**题目描述**

输入一个n行m列的整数矩阵，再输入q个询问，每个询问包含四个整数 $$x1,y1,x2,y2$$，表示一个子矩阵的左上角坐标和右下角坐标。对于每个询问输出子矩阵中所有数的和。

<img src=".\algorithm_prefix.assets\1711590074197.png" alt="1711590074197" style="zoom:50%;" />

```C++
#include <algorithm>
#include <cstring>
#include <iostream>
using namespace std;
const int N = 1010;

int n, m, q;
int a[N][N], s[N][N];

int main()
{
    cin >> n >> m >> q;
    for(int i = 1; i <= n; i++)
        for(int j = 1; j <= n; j++){
            cin >> a[i][j];
            s[i][j] = s[i][j-1] + s[i-1][j] - s[i-1][j-1] + a[i][j];
        }
    
    while( q--){
        int x1, x2, y1, y2;
        cin >> x1 >> y1 >> x2 >> y2;
        cout << s[x2][y2] - s[x2][y1-1] - s[x1-1][y2] + s[x1-1][y1-1] << endl;
    }
        
    return 0;
}
```

==tips==

- 原理：https://leetcode.cn/circle/discuss/UUuRex/
1314. [矩阵区域和](https://leetcode.cn/problems/range-sum-query-2d-immutable/) 1484 
3070. 元素和小于等于 k 的子矩阵的数目 1499
1738. [找出第 K 大的异或坐标值](https://leetcode.cn/problems/find-kth-largest-xor-coordinate-value/) 1671
1292. [元素和小于等于阈值的正方形的最大边长](https://leetcode.cn/problems/maximum-side-length-of-a-square-with-sum-less-than-or-equal-to-threshold/) 1735
221. 最大正方形
1277. 统计全为 1 的正方形子矩阵
1504. 统计全 1 子矩形 1845

**输入**

```
3 4 3
1 7 2 4
3 6 2 8
2 1 2 3
1 1 2 2
2 1 3 4
1 3 3 4
```

**输出**

```
17
27
21
```

**模板注释版**

```C++
#include <algorithm>
#include <cstring>
#include <iostream>
using namespace std;
const int N = 1010;

int n, m, q;
int a[N][N], s[N][N];

int main()
{
    cin >> n >> m >> q;
    for(int i = 1; i <= n; i++)
        for(int j = 1; j <= n; j++){
            cin >> a[i][j];
            // s[i][j]表示从下标a[1][1]到a[i][j]的总和
            // 笔试时候建议用这个模板，可以避免处理边界问题
            // 而且好理解
            s[i][j] = s[i][j-1] + s[i-1][j] - s[i-1][j-1] + a[i][j];
        }
    
    while( q--){
        // 输入点坐标 (x1,y1)  (x2,y2)
        int x1, x2, y1, y2; 
        cin >> x1 >> y1 >> x2 >> y2;
        cout << s[x2][y2] - s[x2][y1-1] - s[x1-1][y2] + s[x1-1][y1-1] << endl;
    }
        
    return 0;
}
```



## 前缀异或和

1. 异或和的具体推导，见leetcode官方题解。结论是：
   $$
   Q(left,right) = xor[left]~~~\oplus ~~~xor[right+1]\\
   xor[i]=a[0] ~\oplus ~a[1] ~\oplus~a[2] ~\oplus... ...~\oplus~a[i-1]
   $$
   
2. [1177. 构建回文串检测](https://leetcode.cn/problems/can-make-palindrome-from-substring/) ==比较难，需要多看几遍==

3. [1542. 找出最长的超赞子字符串](https://leetcode.cn/problems/find-longest-awesome-substring/)

4. [1915. 最美子字符串的数目](https://leetcode.cn/problems/number-of-wonderful-substrings/)，[题解](https://leetcode.cn/problems/number-of-wonderful-substrings/solution/qian-zhui-he-chang-jian-ji-qiao-by-endle-t57t/)

## 前后缀

1. 除自身以外数组的乘积：https://leetcode.cn/problems/product-of-array-except-self



##前后缀最大值

前、后缀最大值

- `pre_max[i] = max(pre_max[i-1], nums[i])`，计算下标范围 `i~n-1`的最大值

  ```python
  suf_max = [0] * (n + 1)
  for i in range(n-1, -1, -1):
      suf_max[i] = max(suf_max[i+1], nums[i])
  ```

- `suf_max[i] = max(suf_max[i+1], nums[i])`，，计算下标范围 `0~i`的最大值

  ```python
  pre_max = [0] * (n + 1)
  for i in range(n):
      pre_max[i+1] = max(pre_max[i], nums[i])
  pre_max = pre_max[1:]
  ```

-  [2874. 有序三元组中的最大值 II](https://leetcode.cn/problems/maximum-value-of-an-ordered-triplet-ii/) 

- [1014. 最佳观光组合](https://leetcode.cn/problems/best-sightseeing-pair/)

  ```python
  class Solution:
      def maximumTripletValue(self, nums: List[int]) -> int:
          n = len(nums)
          suf_max = [0] * (n + 1)
          for i in range(n - 1, 1, -1):
              suf_max[i] = max(suf_max[i + 1], nums[i])
          ans = pre_max = 0
          for j, x in enumerate(nums):
              ans = max(ans, (pre_max - x) * suf_max[j + 1])
              pre_max = max(pre_max, x)
          return ans
  ```

  

##相关笔试题

灵神前缀和题单：https://leetcode.cn/problems/range-sum-query-immutable/solutions/2693498/qian-zhui-he-ji-qi-kuo-zhan-fu-ti-dan-py-vaar/

==主要是为了整理思路，明白该用什么套路、数据结构。==

1、小红劈字符串

https://mp.weixin.qq.com/s/so06zFVMpwc_Ea44wPT9Xg

> 思路：前缀和
>
> 1. 枚举各个下标，统计该下标之前所有的r \ e \ d的字符数量
> 2. 分别判断左子串、右子串是否为好串，如果是，则结果++



2、美团03.30 T2

https://mp.weixin.qq.com/s/oacs8B0oAO2H66WlzltZDw

> 一个前后缀分解的题型，可以把数组拆分为三部分做操作：[0, i-1] [i, i] [i+1, n-1]
>
> - 对于f(i)而言，它的值为   max(left_max, 2*a[i], right_max)
> - 定义两个数组，分别表示区间[0-i-1]的最大值，[i+1, n-1]的最大值
> - 计算 max(left_max, 2*a[i], right_max)



## 面试相关

1. RBBRBR，每次操作可以交换一个R或B，至少操作多少次可使R左侧无B，B右侧无R？

   前缀和：分别统计截至第i个点，前面有多少的R、多少B

2. 













