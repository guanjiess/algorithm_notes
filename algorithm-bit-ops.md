##位运算基本操作

1. 与   &

2. 或   | 

3. 非   ~



## 面试

1. 对于整数n，是否存在一个数k，使得2^k = n？

   ```C++
   int cnt = 0;
   while(n){
       cnt ++;
       n >> 1;
   }
   return cnt > 0;
   ```

   ```C++
   return n & (n-1)
   ```

   

2. 















## bulitin

###python





###C++

-  __builtin_popcount，统计整数二进制表示中1的数量

  ```C++
   __builtin_popcount(lower & upper & ~invalid)
  ```

- 