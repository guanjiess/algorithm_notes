##一维前缀和

描述：给定一个数组，求数组指定范围内的和

<img src="E:\master2\coding_notes\DSA\algorithm_prefix.assets\1711589770286.png" alt="1711589770286" style="zoom: 50%;" />

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
    for(int i = 1; i <= n; i++){
        s[i] = s[i-1] + a[i];
    }
    while(m--){
        int l;
        int r;
        cin >> l >> r;
        printf("%d\n", s[r] - s[l-1]);
    }
    return 0;
}
```

### 前缀+哈希

- https://leetcode.cn/problems/make-sum-divisible-by-p/
- https://leetcode.cn/problems/binary-subarrays-with-sum/description/
- https://leetcode.cn/problems/subarray-sum-equals-k/description/
- https://leetcode.cn/problems/subarray-sums-divisible-by-k/



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

- 笔试为了避免边界问题，**下标从1开始**
- 原理：https://leetcode.cn/circle/discuss/UUuRex/
- https://leetcode.cn/problems/count-square-submatrices-with-all-ones/

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



## 前后缀

1、除自身以外数组的乘积：https://leetcode.cn/problems/product-of-array-except-self











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















