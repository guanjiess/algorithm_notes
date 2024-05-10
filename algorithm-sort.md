##应用场景

可视化参考：https://www.cs.usfca.edu/~galles/visualization/ComparisonSort.html



##快排

###codes

快排流程

主要思想：基于分治

1. 确定分界点：常见的三种方法，取临界值x =  左边界、中间点、右边界
2. 调整区间，使所有小于
3. 递归处理左右两段 

**C++模板**

```C++
#include <iostream>
using namespace std;
const int N = 1e6 + 10;
int n;
int q[N];

void quick_sort(int q[], int l, int r){
    if(l >= r) return;
    int x = q[l+r>>1], i = l - 1, j = r + 1;
    while(i < j){
        do i++; while(q[i] < x);
        do j--; while(q[j] > x);
        if(i < j){
            swap(q[i], q[j]);
		}
    }
    quick_sort(q, l, j);
    quick_sort(q, j+1, r);
}

int main(){
    scanf("%d", &n);
    for(int i = 0; i < n; i++) scanf("%d", &q[i]);
    quicl_sort(q, 0, n-1);
    
    for(int i = 0; i < n; i++) printf("%d ", q[i]);
    
    return 0;
}
```

**python模板**

```python
n, k = map(int, input().split())
arr = list(map(int, input().split()))

def quick_select(arr, l, r, k):
    if l >= r:
        return None
    i = l - 1
    j = r + 1
    x = arr[(l + r) // 2]
    while i < j:
        while True:
            i += 1
            if arr[i] >= x:
                break 
        while True:
            j -= 1
            if arr[j] <= x:
                break 
        if i < j:
            arr[i], arr[j] = arr[j], arr[i]
    if j < k - 1:
        quick_select(arr, j + 1, r, k)
    else:
        quick_select(arr, l, j, k)

quick_select(arr, 0, n - 1, k)
print(str(arr[k - 1]))
```



###特点

- 快排在最坏的情况下时间复杂度是O(n^2)，平均是O(nlog(n))





###快速选择

时间复杂度比快排低，平均时间复杂度为O(n)。证明如下（第一层遍历n，第二层遍历n/2... ...）
$$
O = n +\frac{n}{2}+\frac{n}{4}+\frac{n}{8}+......+ \le2n
$$

```C++
#include <iostream>

using namespace std;

const int N = 1000010;

int q[N];

int quick_sort(int q[], int l, int r, int k)
{
    if (l >= r) return q[l];

    int i = l - 1, j = r + 1, x = q[l + r >> 1];
    while (i < j)
    {
        do i ++ ; while (q[i] < x);
        do j -- ; while (q[j] > x);
        if (i < j) swap(q[i], q[j]);
    }

    if (j - l + 1 >= k) return quick_sort(q, l, j, k);
    else return quick_sort(q, j + 1, r, k - (j - l + 1));
}

int main()
{
    int n, k;
    scanf("%d%d", &n, &k);

    for (int i = 0; i < n; i ++ ) scanf("%d", &q[i]);

    cout << quick_sort(q, 0, n - 1, k) << endl;

    return 0;
}
```

https://www.acwing.com/solution/content/2098/

### 改进

- 双路
- 单路
- 到一定级数之后用归并替代



##归并排序

###codes

归并排序流程

1. 分两半
2. 递归排序
3. 归并，把左右两段有序数据合并。（时间复杂度O(N)，重点、比较困难）

题目

```C++
#include <iostream>
using namespace std;
const int N = 1e6 + 10;
int q[N], tmp[N];

void merge_sort(int q[], int l, int r){
    if(l >= r) return;
   	int mid = l +r >> 1;
    
    merge_sort(q, l, mid);
    merge_sort(q, mid+1, r);
    
    // 归并
    int k = 0, i = l, j = mid + 1;
    while(i <= mid && j <= r){
        if(q[i] <= q[j]) tmp[k++] = q[i++];
        else tmp[k++] = q[j++];
    }
    while(i <= mid) tmp[k++] = q[i++];
    while(j <= r) tmp[k++] = q[j++];
    // 从临时数组中取出排序后的数据
    for(i = l, j = 0; i <= r; i++, j++) q[i] = tmp[j];
}

int main()
{
    int n;
    scanf("%d", &n);
    for (int i = 0; i < n; i ++ ) scanf("%d", &a[i]);

    merge_sort(a, 0, n - 1);

    for (int i = 0; i < n; i ++ ) printf("%d ", a[i]);

    return 0;
}
```



###特点

- 快排是不稳定的，归并排序是稳定的。
- 归并时间复杂度是O(nlog(n))：每一层的归并过程是O(n)的计算量，总计有log(n)层，所以总的复杂度是O(nlog(n))

###求逆序对

虽然y总距离说有三种情况，但实际上只有中间黄色的一种情况。为了求逆序对的数量，在宏观上划分为了三种情况

- 分布在左半边的逆序对数量
- 分布在右半边的逆序对数量
- 穿插在二者中间的逆序对数量
- 总数 =   左  + 右  + 穿插
- 而夹在二者中间的逆序对数量可以在归并排序的归并过程中求得。设左侧为a[i]，右侧为a[j]，如果a[i]>a[j]，那么a[j]对应的逆序对数量为   mid - i + 1。

<img src="E:\master2\coding_notes\DSA\algorithm-sort.assets\1715087052946.png" alt="1715087052946" style="zoom: 67%;" />

```C++
#include<iostream>
using namespace std;
typedef long long LL ;  //定义long long 类型起个自定义名称！
const int N = 1e5 + 10;
int a[N],tmp[N];

LL merge_sort(int a[],int l,int r){
    if(l >= r) return 0;
    int mid = l + r >> 1 ;
    LL res = merge_sort(a,l,mid) + merge_sort(a,mid + 1 , r); //递归排序！
    // 注意i和j的顺序初始值
    int  k = 0 , i = l ,j = mid + 1;  
    while(i <= mid &&  j <= r){         //归并，整理逆序对的过程
        if(a[i] <= a[j]) tmp[k++] = a[i++];
        else{
            res += mid - i + 1;  //满足逆序对条件
            tmp[k++] = a[j++] ;
        } 
    }
    while(i <= mid) tmp[k++] = a[i++];
    while(j <= r) tmp[k++] = a[j++];

    for(int i = l , j =0; i <= r ; i++ , j++) a[i] = tmp[j];
    return res;
}
int main(){
    int n;
    scanf("%d",&n);
    for(int i = 0 ; i < n ; i ++) scanf("%d",&a[i]);
    cout << merge_sort(a,0,n-1) << endl ;
    return 0;
}
```



### 改进

1、https://leetcode.cn/problems/merge-intervals/



## 冒泡



