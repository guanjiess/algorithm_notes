## 应用场景





## 板子&基础操作

链表节点的==增删改查==

**什么时候需要dummy node?**涉及头节点时，可以考虑删除。



==C++==



==python==









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

