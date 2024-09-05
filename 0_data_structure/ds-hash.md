##应用场景

git

各种加密算法



##概述

哈希的主要思想：通过一定的计算方式把一个元素化为整数，并且使得整数可以尽量唯一地表示该元素。 
$$
Hash(key)~~~=~~~value
$$
对于hash来说，**最常见的应用**就是把key作为数组下标，以代替该元素。

##Hash-table

The most popular use for hashing is the implementation of hash tables. A hash table stores key and value pairs in a list that is accessible through its index.

Hash tables support functions that include the following:

- insert (key, value)
- get (key)
- delete (key)



典型的应用：git



##双变量问题

### 思路

总的原则是，遍历右边、维护左边（先查询、再更新）。这样每一次查询操作可以获得的信息量是O(n)级别的，而逐个查询每次获取信息量是O(1)级别的，极大提高了效率。

###相关题目

1. https://leetcode.cn/problems/two-sum/

2. https://leetcode.cn/problems/contains-duplicate-ii/description/

3.  [1679. K 和数对的最大数目](https://leetcode.cn/problems/max-number-of-k-sum-pairs/)   Java feature  

   ```Java
   getOrDefault();
   merge(x, 1, Integer::sum)
   ```

   >在 Java 中使用 `Map` 时, 可以使用 `merge()` 方法来更新或添加键值对. 这个方法接受三个参数:
   >
   >1. 要更新或添加的键 (`x`)
   >2. 要合并的新值 (`1`)
   >3. 一个合并函数 (`Integer::sum`)
   >
   >这行代码的作用是:
   >
   >1. 如果 `Map` 中不存在键 `x`, 则添加一个新的键值对 `(x, 1)`.
   >2. 如果 `Map` 中已经存在键 `x`, 则使用合并函数 `Integer::sum` 将新值 `1` 与旧值相加, 更新键 `x` 的值.
   >
   >也就是说, 这行代码会将键 `x` 的值增加 1. 如果键 `x` 不存在, 则添加一个值为 1 的新键值对.

4. **时长被60整除的歌曲** https://leetcode.cn/problems/pairs-of-songs-with-total-durations-divisible-by-60

5. https://leetcode.cn/problems/number-of-good-pairs/description/

6. **美丽下标对** https://leetcode.cn/problems/number-of-beautiful-pairs/solutions/

7. 复制链表：https://leetcode.cn/problems/copy-list-with-random-pointer/

8.  [2971. 找到最大周长的多边形](https://leetcode.cn/problems/find-polygon-with-the-largest-perimeter/)  

9.  [2874. 有序三元组中的最大值 II](https://leetcode.cn/problems/maximum-value-of-an-ordered-triplet-ii/)    ==amazing==



