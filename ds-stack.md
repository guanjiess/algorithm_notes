## 应用场景

所有的数据后进先出场景。





##栈

1、有效括号：https://leetcode.cn/problems/valid-parentheses/

2、字符串解码：https://leetcode.cn/problems/decode-string

> 我的思路，
> 题目的难点在于怎么处理嵌套的括号。每对括号内有一个字符串，每对括号外有一个字符串s2、以及括号内字符串s1，及其重复次数k。整体关系可以描述为  res = s1 + k s2
>
>
> 1. 左括号：意味着要开始处理一个新的字符串，此时把外侧字符串和重复次数入栈
> * 维护一个数字，以处理嵌套情况
> * 维护一个字符串，以保留结果字符串
> * 遇到左括号，需要入栈并清空
> 2. 右括号：处理完毕括号内的字符串。需要与外侧字符串合并。
> * 取栈顶元素并弹出
> * 拼接字符串

## 括号

- 左括号入栈
- 右括号出栈







## 栈与递归







## 单调栈

==适用==：求解上一个或者下一个更大、更小元素。

739 每日温度 https://leetcode.cn/problems/daily-temperatures/solution/shi-pin-jiang-qing-chu-wei-shi-yao-yao-y-k0ks/ 

==思路==：及时去掉无用数据，保证栈中数据有序。

从右到左



从左到右