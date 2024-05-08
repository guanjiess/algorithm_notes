## 应用场景

1、实现动态的数据结构，如动态数组、队列、栈

2、实现缓存的LRU算法

3、实现文件系统

4、管理内存

## 板子&基础操作

链表节点的==增删改查==

**什么时候需要dummy node?**涉及头节点时，可以考虑删除。

==C++==

```C++
struct ListNode{
	int val;
    ListNode* next;
    ListNode(int x): val(x), next(nullptr){}
};
```

==python==

```python
class ListNode:
    def __init__(self, x):
        self.val = x
        self.next = None
```



##删除链表

1. 删除重复元素https://leetcode.cn/problems/remove-duplicates-from-sorted-list/description/
2. 删除重复元素2https://leetcode.cn/problems/remove-duplicates-from-sorted-list-ii/
3. 移除指定元素 https://leetcode.cn/problems/remove-linked-list-elements/ 
4. 删除倒数第n个节点  https://leetcode.cn/problems/remove-nth-node-from-end-of-list
5. 移除链表元素 https://leetcode.cn/problems/remove-linked-list-elements/description/



##反转链表

- https://leetcode.cn/problems/reverse-linked-list/

  > 需要三个变量执行翻转操作，current为当前节点、next保存current的下一个节点、pre为前序节点

- 两数相加：https://leetcode.cn/problems/add-two-numbers-ii/ 

- https://leetcode.cn/problems/swap-nodes-in-pairs

- https://leetcode.cn/problems/reverse-linked-list-ii

  > 把left到right区间内的链表进行翻转，最终有这样的指向关系
  >
  > <img src=".\algorithm-lists.assets\1712800331594.png" alt="1712800331594" style="zoom:50%;" />
  >
  > 需要保留的变量：left之前的节点p0，p0最终指向right节点，left节点指向right右侧节点

- https://leetcode.cn/problems/reverse-nodes-in-k-group/

  > 和前两道题一脉相承，关键在于保存每一个分组头节点的前导节点p0

   

==容易犯错的点==

在翻转链表时，新的链表头节点已经成为了pre，所以翻转链表要返回的是pre节点

```C++
 ListNode* reverse(ListNode* head){
        ListNode* dummy = new ListNode(0, head);
        ListNode* cur = dummy->next;
        ListNode* pre = nullptr;
        ListNode* next = nullptr;
        while(cur){
            next = cur->next;
            cur->next = pre;
            pre = cur;
            cur = next;
        }
        //总是这里出错，
        return pre;
}
```



## 环形链表

1. 快慢指针 https://leetcode.cn/problems/linked-list-cycle-ii

   <img src=".\algorithm-lists.assets\1712887841369.png" alt="1712887841369" style="zoom:50%;" />

   > 解释：快指针一次走两步，慢指针一次走一步，有两个基本事实
   >
   > 1. 快慢指针一定在环内相遇，相遇时慢指针还没有走完一整圈。
   >
   >    - 慢指针距离：a+b
   >    - 快指针距离：a+b+c
   >    - 环长：b+c
   > 2. 满足此关系：   2（a+b） = a + b + k (b+c)---> a - c = (k-1)(b+c)。解读：头节点到入口处的距离，比快慢指针相遇点到环入口的距离，多了整数倍的环长，也就是说二者一定会在入口处相遇。
   >    - 若k=1，那就是  a =  c
   >    - k = 2，那就是a到入口处的距离比slow到入口处的距离多了整整一圈。以此类推
   >
> 也就是说如果slow从相遇点开始向前走，头节点同时也向前走，那么他们一定会在入口点相遇。

   

## 相交链表

https://leetcode.cn/problems/intersection-of-two-linked-lists

两种思路

1. 开一个哈希表，记录链表中的节点。
* 首先遍历其中一个链表，将其所有节点记录其中
* 在遍历第二个链表时，查询到的第一个存储在哈希表中的节点，即为交点

2.拉通对齐：用两个指针同时遍历链表a和b，直到二者相遇。
* 设链表a、b公共部分长度为c，非公共部分长度分别为a\b
* 我们的逻辑是，每次循环让一个指针前进一步，遍历完当前链表后继续遍历下一个链表
* 那么指针a到达交点时，经历的路程是 a+b+c，指针b到达交点时，经历的路程也是 a+b+c，且每次循环两个指针都会向前走一步，所以如果两个链表相交，两个指针必然相遇。



## 双向链表-LRU

```C++
class Node{
public:
    int key, val;
    Node* prev;
    Node* next;
    //记得node给个初始值
    Node(int k = 0, int v = 0): key(k), val(v){}
};

class LRUCache{
private:
    unordered_map<int , Node*> key_to_value;
    int capacity;
    Node* dummy;
    void remove(Node* x){
        x->prev->next = x->next;
        x->next->prev = x->prev;
    }

    //初始化dummy的prev next分别指向自身，可保证形成一个环形双向链表
    void put_front(Node* x){
        x->prev = dummy;
        x->next = dummy->next;
        x->prev->next = x;
        x->next->prev = x;
    }

    Node* get_node(int key){
        auto it = key_to_value.find(key);
        if(it == key_to_value.end()){
            return nullptr;
        }
        auto node = it->second;
        remove(node);
        put_front(node);
        return node;
    }

public:
    LRUCache(int capacity) : capacity(capacity), dummy(new Node()){
        dummy->prev = dummy;
        dummy->next = dummy;
    }
    
    int get(int key){
        auto node = get_node(key);
        return node ? node->val : -1;
    }

    void put(int key, int val){
        auto node = get_node(key);
        if(node){
            node->val = val;
            return ;
        }
        key_to_value[key] = node = new Node(key, val);
        put_front(node);
        if(key_to_value.size() > capacity){
            auto back_node = dummy->prev;
            //哈希表根据key来做判决
            key_to_value.erase(back_node->key);
            remove(back_node);
            delete back_node;
        }
    }
};
```



==核心操作==

1. get(int key)，从缓存中取出key对应的数据，因为当前对key的操作是最新的，所以需要它放在最前面。
2. put(int key, int val)，向缓存中存储数据 (key, val)，并且把它放在最上方。因为向缓存中添加数据，可能导致容量超过上限，所以需要删除尾结点。
3. 为了实现O(1)的时间复杂度，在进行增删的时候

==为什么要用双向链表？为什么要用哈希？==

1、为什么用双链表

- 单链表在增删层面的时间复杂度是O(1)的，但是**LRU算法隐含一个要求，每次对一个结点做操作时需要把结点移动到缓存的头部**，如果要把一个单链表的结点移动到头部位置，由于单链表结点没有前驱，所以需要从头到尾找到前驱之后才能把操作的结点 cur 断开，重连到头部。
  - 例如：1 -> 2 -> 3 -> 4 -> 5， 要把节点值为 3 的节点移动到链表的头部， 需要从头部开始遍历链表，直到找到节点 3 的前驱节点2为止  
  - 修改节点 2 的指针指向节点 4，将节点 3 的指针指向链表的头节点（节点 1），然后将链表的头节点指向节点 3，得到3 -> 1 -> 2 -> 4 -> 5 
  - 最坏的情况下，需要遍历全部结点，时间复杂度是O(N)
- 使用双链表有前驱、后继两个方向的结点，任何位置的增删都可以在O(1)时间内完成。可以在O(1)的时间内把结点移动到头结点。

2、为什么用哈希？

使用哈希能用O(1)的复杂度实现get()方法，可以直接根据key来索引链表结点的指针。

3、整体过程

就像是在抽一摞书，放一本书时默认放在最上面（如果已经有的话就把它拿上来），取完一本书用完之后再放到最上面。涉及的操作有

- 双链表的结点删除
- 双链表的头部结点插入
- 取出双链表中的某结点