##堆

堆是一种完全二叉树，除了最底层外，其他层的节点都是满的，且最底层的节点都尽可能地靠左排列。通常用来实现优先队列，分为最大堆和最小堆两种类型。

1. 最大堆：父节点的值大于子节点的值，即堆的根节点拥有最大值。
2. 最小堆：父节点的值小于子节点，堆的根节点有最小值。



==常见面试题==

## 堆排序

1、topk问题：https://leetcode.cn/problems/kth-largest-element-in-an-array/

- 思路，建立一个小顶堆，在其内部保留最大的K个数，那么堆顶元素就是第K大元素。


```C++
int findKthLargest(vector<int>& nums, int k) {
        // 建 k 个数的小堆 --- TOPK问题
        priority_queue<int,vector<int>,greater<int>> pq(nums.begin(),nums.begin()+k);

        for(int i = k;i < nums.size();i++)
        {
            if(nums[i] > pq.top())
            {
                pq.pop();
                pq.push(nums[i]);
            }
        }

        return pq.top();
}
```

2、雇佣工人：https://leetcode.cn/problems/total-cost-to-hire-k-workers/。灵神的题解非常优雅。

3、合并k个升序链表：https://leetcode.cn/problems/merge-k-sorted-lists/  

4、雇佣K名工人的最低成本：https://leetcode.cn/problems/minimum-cost-to-hire-k-workers





## 库

==python==

- heapq，默认为小顶堆，常用的操作函数包括

`heapq` 模块是 Python 中用于实现堆数据结构的工具，提供了一些常用的功能来对堆进行操作。以下是 `heapq` 模块中最常用的几个功能：

1. 创建堆：使用 `heapq.heapify(heap)` 函数可以将一个列表 `heap` 转换为堆。这个函数会重新排列列表中的元素，使其满足堆的性质。
2. 压入元素：使用 `heapq.heappush(heap, item)` 函数可以向堆 `heap` 中压入一个新的元素 `item`。元素会被插入到合适的位置以保持堆的性质。
3. 弹出顶部元素：使用 `heapq.heappop(heap)` 函数可以弹出并返回堆 `heap` 的顶部元素，即最小元素。弹出后，堆会重新调整以保持堆的性质。
4. 替换堆顶元素：使用 `heapq.heapreplace(heap, new_item)` 函数可以替换堆 `heap` 的顶部元素为新元素 `new_item`，并保持堆的性质。新元素可能会被放置到合适的位置。
5. 获取堆顶元素：可以直接通过索引访问堆顶元素，例如 `heap[0]`，来获取堆的顶部元素，即最小元素。

==C++==

在C++中，通常使用 `` 头文件中的 `priority_queue` 类来实现堆。`priority_queue` 是一个优先队列容器，它基于堆数据结构实现，可以自动将元素按照一定的顺序进行排序。 

在C++中，`priority_queue` 是一个标准库容器，用于实现优先队列，基于堆数据结构。下面是 `priority_queue` 的基本使用方法：

1. 包含头文件：首先需要包含 `` 头文件。

```
#include <queue>
```

1. 创建 `priority_queue` 对象：可以使用模板来创建一个 `priority_queue` 对象，指定存储元素的类型和比较函数。

```
std::priority_queue<int, vector<int>, greater<>> pq; // 创建一个存储整数的优先队列。底层实现是vector<int>，greater<>将pq定义成了一个小顶堆。（priority_queue默认是个大顶堆）
```

1. 插入元素：可以使用 `push()` 方法向优先队列中插入元素。

```
pq.push(10); // 插入元素 10
pq.push(20); // 插入元素 20
```

1. 访问元素：可以使用 `top()` 方法访问优先队列中的顶部元素，即最高优先级的元素。

```
int topElement = pq.top(); // 获取优先队列中的顶部元素
```

1. 删除元素：可以使用 `pop()` 方法删除优先队列中的顶部元素。

```
pq.pop(); // 删除优先队列中的顶部元素
```

1. 获取队列大小：可以使用 `size()` 方法获取优先队列中元素的数量。

```
int size = pq.size(); // 获取优先队列的大小
```

通过以上操作，您可以使用 `priority_queue` 实现优先队列的基本功能，自动根据元素的优先级进行排序和访问。