##二分查找

看算法笔记，**重点注意数据区间的问题**。

- 对值域二分：直接对值域做查找
- 对索引二分：一般用于对有序数组的索引

用while循环实现时，判定条件是括号内的区间有效性质。

```
left < right  or left <= right
```

> 算法笔记总结很好。

==用来求解最大化最小值 or  最小化最大值==，用来解决有序序列中第一个满足某条件元素的位置。

###不同区间形式

- leetcode34: https://leetcode.cn/problems/find-first-and-last-position-of-element-in-sorted-array/

重要的是根据target和mid的比较结果，更新得到**最新的查询区间**

1、左闭右闭

```C++
int lower_bound(vector<int>& nums, int target){
        int l = 0, r = nums.size()-1;  // [0, n-1]
        while(l <= r){ 		// 区间不为空
            int mid = l + (r-l)/2;
            if(nums[mid] < target){
                l = mid + 1;// 目标在区间[mid+1, right]
            } else{
                r = mid - 1;// 目标在区间[left, mid-1]
            }
        }
        return l; // l = r+1
}
```

1. 初始区间：`l = 0, r = size - 1`
2. 循环条件：区间不为空  `l <= r`
3. 终止情况：`l = r + 1`
   - 最终L指针停在了R的右边。



2、左闭右开

```C++
// 左闭右开写法
    int lower_bound2(vector<int>& nums, int target){
        int l = 0, r = nums.size(); //[0,n)
        while(l < r){				// 区间不为空
            int mid = l + (r-l)/2;  //避免溢出
            if(nums[mid] < target){
                l = mid + 1; 		// [mid+1, r) 
            } else{
                r = mid;  			// [l, mid)
            }
        }
        return l;  // l = r
    }
```

最终L和R停在了一起。

3、左开右开

```C++
int lower_bound3(vector<int>& nums, int target){
        int l = -1, r = nums.size();
        while(l+1 < r){
            int mid = l + (r-l)/2;
            if(nums[mid] < target){
                l = mid;
            } else{
                r = mid;
            }
        }
        return r; // l+1 = r
}
```



### 模板1-递增序列找到第一个≥

也就是找到第一个大于某个x关系的最小值。

```C++
// 左闭右开 
int lower_bound(vector<int>& nums, int x){
    int l = -1, r = nums.size();
    while(l+1 < r){
        int mid = l + (r-l)/2;
        if(nums[mid] >= x){
            r = mid;  //(left, mid)
        } else{
            l = mid; // (mid, right)
        }
    }
    // 跳出循环后，表示第一个大于x的数字
    if(nums[l] < x) return -1;
    else return l;
}
```



### 模板2-递增序列中找到第一个＞

```C++
int lower_bound(vector<int> &nums, int x){
	int l = -1, r = nums.size();
    while(l+1 < r){
        int mid = l + (r-l)/2;
        if(nums[mid] > x){
            r = mid;   // (left, mid)
        } else{
            l = mid; // (mid+1, right)
        }
    }
    if(nums[l]  > x) return l;
    else return -1;
}
```

### ＜

- 小于x，可以视作找大于等于x左边的那个数

```
int index = lower_bound(nums, x) - 1;
```

### ≤

- 小于等于x，可以是做是x+1左边的那个数

```C++
int index = lower_bound(nums, x+1) - 1;
```



##循环不变量

关键不在二分区间内的元素有什么性质，关键在于二分区间外的元素有什么性质。

- 在循环的过程中，保持不变的性质
- L的左边恒小于x，R的右边恒大于x
- 不了解情况的范围框在区间内

> https://www.bilibili.com/video/BV1AP41137w7 评论区
>
> 我是在看了这篇文章，https://blog.csdn.net/groovy2007/article/details/78309120，里那句“关键不在于区间里的元素具有什么性质，而是区间外面的元素具有什么性质。”之后醍醐灌顶，建立了我自己的二分查找心智模型，和up主的有些类似。
>
> 也就是**看最终左右指针会停在哪里。**
> 如果我们要找第一个大于等于x的位置，那么我就假设L最终会停在第一个大于等于x的位置，R停在L的左边。
> 这样按照上面那句话，可以把循环不变式描述为“**L的左边恒小于x，R的右边恒大于等于x**”，这样一来，其他的各种条件就不言自明了。
> 比如**循环条件肯定是L小于R**，因为我假设R停在L的左边。
> 而L和R转移的时候，根据**循环不变式**，如果mid小于x，肯定要令L等于mid+1，如果大于等于x，就令R等于mid-1。
> 至于初始的时候L和R怎么选，也是看循环不变式，只需要保证初始L和R的选择满足“L的左边恒小于x，R的右边恒大于等于x”，并且不会出现越界的情况即可，L必为0，因为0左边可以看作负无穷，恒小于x，R取第一个一定满足条件的（防止mid取到非法值），例如n-1（n开始可以看作正无穷，恒大于等于x，如果保证x在数组里可以选择n-2，其实大于等于n也满足不变式，但是mid可能会取非法值），而且这样一来即使是搜索数组的某一段，也可以很方便根据这个条件地找到初始位置。
>
> 如果**假设L最终会停在第一个大于等于x的位置，R停在L的位置，那么循环不变式就是“L的左边恒小于x，R以及R的右边恒大于等于x”**，这样的话，循环条件就是L等于R的时候退出；转移的时候R=mid；初始时，一般取R=n（如果保证x在数组里，也可以取n-1）。
>
> 其他的情况也类似，比较直观的推导方法就是在要找的位置的分界处（比如在第一个大于等于x的位置后面）画一条线，然后假定L和R最终会停在这条线的左边还是右边，接着倒推各种条件即可。 

- [704. 二分查找](https://leetcode.cn/problems/binary-search/)

```python
def lower_bound3(nums: List[int], target: int) -> int:
    left, right = -1, len(nums)  # 开区间 (left, right)
    while left + 1 < right:  # 区间不为空
        mid = (left + right) // 2
        # 循环不变量：
        # nums[left] < target
        # nums[right] >= target
        if nums[mid] < target:
            left = mid  # 范围缩小到 (mid, right)
        else:
            right = mid  # 范围缩小到 (left, mid)
    return right  # 或者 left+1

```

- [744. 寻找比目标字母大的最小字母](https://leetcode.cn/problems/find-smallest-letter-greater-than-target/)

```python
class Solution:
    def nextGreatestLetter(self, letters: List[str], target: str) -> str:
        l, r = -1, len(letters)
        while l+1 < r:
            # 循环不变量 
            # letters[left] <= target
            # letters[right] > target
            mid = l + (r-l)//2
            if letters[mid] <= target:
                l = mid
            else:
                r = mid
            print(r)
        if r < len(letters) and letters[r] > target:
            return letters[r]
        return letters[0]
```

- [2856. 删除数对后的最小数组长度](https://leetcode.cn/problems/minimum-array-length-after-pair-removals/)

- [2563. 统计公平数对的数目](https://leetcode.cn/problems/count-the-number-of-fair-pairs/)   **一种类似于消消看的trick**

- [1818. 绝对差值和](https://leetcode.cn/problems/minimum-absolute-sum-difference/) 

  > 用二分查找的方式找到nums1中和nums2中差别最小的元素去替换。





##C++库

- upper_bound：查找**第一个大于**给定值的元素位置。  $\ge$的下界
- lower_bound：查找**第一个大于等于**给定值的元素位置  $>$的下届



## python库

**bisect库**：用来处理已排序序列。

- `bisect.bisect_left(a, x, lo=0, hi=len(a))`: 在已排序的列表 `a` 中查找元素 `x` 应该**插入的位置**，返回插入位置的索引。如果元素已经存在，则返回其左侧位置索引。

- `bisect_left(array, x, left, right)`，对区间`[left, right)`做二分搜索

  ````python
  import bisect
  
  a = [1, 2, 4, 5, 6]
  x = 3
  
  insert_pos = bisect.bisect_left(a, x)
  print(insert_pos)
  ````

  - 等价于C++中的  `lower_bound()`，返回  $\ge x$的元素位置的下界。
  - The returned insertion point *ip* partitions the array *a* into two slices such that `all(elem < x for elem in a[lo : ip])` is true for the left slice and `all(elem >= x for elem in a[ip : hi])` is true for the right slice. 

- `bisect.bisect_right(a, x, lo=0, hi=len(a))`: 在已排序的列表 `a` 中查找元素 `x` 应该**插入的位置**，返回插入位置的索引。如果元素**已经存在，则返回其右侧位置索引**。

  - 等价于C++中的  `upper_bound()`，返回  $>x$的元素位置的下界。即第一个大于 x 元素的下标位置。
  - `bisect_right(array, x, left, right)`，对区间`[left, right)`做二分搜索
  - The returned insertion point *ip* partitions the array *a* into two slices such that `all(elem <= x for elem in a[lo : ip])` is true for the left slice and `all(elem > x for elem in a[ip : hi])` is true for the right slice. 





## 红蓝染色法

红蓝实际上是循环不变量，例如

> 红色表示最小值左侧，蓝色表示最小值及其右侧。----最小值左侧、最小值及其右侧就是一种不随循环变化的稳定性质。

1、while的跳出条件

2、while中的判断条件

3、L和R的最终停留位置关系

4、返回left还是right：根据check条件来定，在满足条件时赋给谁值，就返回谁。




