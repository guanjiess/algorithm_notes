## 应用场景

所有的数据后进先出场景。

先进后出 fist in last out；



### 栈基础

1. [844. 比较含退格的字符串](https://leetcode.cn/problems/backspace-string-compare/) 

2. [1472. 设计浏览器历史记录](https://leetcode.cn/problems/design-browser-history/)  有一点迷糊

3. [946. 验证栈序列](https://leetcode.cn/problems/validate-stack-sequences/)

   ```c++
   class Solution {
   public:
       bool validateStackSequences(vector<int>& pushed, vector<int>& popped) {
           vector<int> pushed2;
           for(int i = 0, j = 0; i < pushed.size(); i++){
               pushed2.push_back(pushed[i]);
               //满足情况，则弹出栈顶
               while(!pushed2.empty() && pushed2.back() == popped[j]){
                   pushed2.pop_back();
                   j ++;
               }
           }
           return pushed2.size() == 0;
       }
   };
   ```

4. [71. 简化路径](https://leetcode.cn/problems/simplify-path/)
5. [155. 最小栈](https://leetcode.cn/problems/min-stack/)

怎么验证一个栈的输出序列是否正确？



### 括号

- 左括号入栈
- 右括号出栈

1. 有效括号：https://leetcode.cn/problems/valid-parentheses/

###表达式解析

1. 字符串解码：https://leetcode.cn/problems/decode-string

> 我的思路，
> 题目的难点在于怎么处理嵌套的括号。每对括号内有一个字符串，每对括号外有一个字符串s2、以及括号内字符串s1，及其重复次数k。整体关系可以描述为  res = s1 + k s2
>
>
> 1. 左括号：意味着要开始处理一个新的字符串，此时把外侧字符串和重复次数入栈
>
> * 维护一个数字，以处理嵌套情况
> * 维护一个字符串，以保留结果字符串
> * 遇到左括号，需要入栈并清空
>
> 2. 右括号：处理完毕括号内的字符串。需要与外侧字符串合并。
>
> * 取栈顶元素并弹出
> * 拼接字符串

###消消看



### 中缀表达式







##栈-模板







## 栈与递归



## 单调栈

>  他向远方望去，无法看到高山背后的矮山，只能看到一座座更高的山峰。 

==适用范围==

- 求解上一个或者下一个更大、更小元素。

### 核心原则

整体思路为构造单调栈、用单调栈计算需要的答案。核心的原则是**及时移除无用数据，保证栈/队列的有序性**。

遍历时，一般有从左到右、从右到左两种遍历方式，思路如下：

```bash
 /**
        几个基本的问题
        1. 单调栈的从左遍历、从右遍历的操作流程
        2. 单调栈中，存的什么，遍历的数据是什么角色 重点在理解栈中数据的角色
        left -> right
        ** 从左到右遍历 -> 取当前元素和栈顶对比，符合条件则更新答案、出栈-> 当前元素入栈
        ** 栈中存储未得到答案的对象，遍历的是候选数据

        right -> left : 从右到左遍历 -> 
        ** 从右到左遍历 -> 取当前元素和栈顶对比，不是答案则持续出栈-> 更新答案-> 当前元素入栈
        ** 栈中存储未得到答案的对象，遍历的是候选数据
         */
        // //单调栈求解，遍历数组
```

### 习题

- [每日温度](https://leetcode.cn/problems/daily-temperatures/solution/shi-pin-jiang-qing-chu-wei-shi-yao-yao-y-k0ks/ )
- [496. 下一个更大元素 I](https://leetcode.cn/problems/next-greater-element-i/)
- [503. 下一个更大元素 II](https://leetcode.cn/problems/next-greater-element-ii/)
- [1019. 链表中的下一个更大节点](https://leetcode.cn/problems/next-greater-node-in-linked-list/) 
-  [962. 最大宽度坡](https://leetcode.cn/problems/maximum-width-ramp/)  
-  [1124. 表现良好的最长时间段](https://leetcode.cn/problems/longest-well-performing-interval/)  ==范例，和962类似，都是求最大子区间长度，理解暂时有问题==
-  [853. 车队](https://leetcode.cn/problems/car-fleet/) 
-  [901. 股票价格跨度](https://leetcode.cn/problems/online-stock-span/)  
- [456. 132 模式](https://leetcode.cn/problems/132-pattern/)
- [1793. 好子数组的最大分数](https://leetcode.cn/problems/maximum-score-of-a-good-subarray/) 

###栈的单调性

栈中元素要保持单调，求解更小值时，栈顶到栈底是递减的；求解更大值，从栈顶到栈底递增。

==关键操作==：怎么确定栈中元素的单调性？

### 遍历顺序

**从左到右遍历**

从左到右遍历时，**被遍历的元素是答案的候选**，栈中元素是未待求解元素。

整个流程大体分为入栈的遍历、出栈的清算两个阶段。以求左侧或者右侧更小值为例：栈中元素从上到下严格按递减排序，当遍历元素x < 栈顶st{-1}时，说明找到了答案，逐个弹出栈顶并更新答案，弹出栈时

- 当前元素x出发了答案的更新，为栈顶元素的右侧更小值
- 栈顶元素的左侧答案为当栈顶下方元素

```Java
public class Code01_LeftRightLess {

	public static int MAXN = 1000001;

	public static int[] arr = new int[MAXN];

	public static int[] stack = new int[MAXN];

	public static int[][] ans = new int[MAXN][2];

	public static int n, r;
	// 输入
	public static void main(String[] args) throws IOException {
		BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
		StreamTokenizer in = new StreamTokenizer(br);
		PrintWriter out = new PrintWriter(new OutputStreamWriter(System.out));
		while (in.nextToken() != StreamTokenizer.TT_EOF) {
			n = (int) in.nval;
			for (int i = 0; i < n; i++) {
				in.nextToken();
				arr[i] = (int) in.nval;
			}
			compute();
			for (int i = 0; i < n; i++) {
				out.println(ans[i][0] + " " + ans[i][1]);
			}
		}
		out.flush();
		out.close();
		br.close();
	}

	// arr[0...n-1]
	public static void compute() {
		r = 0;
		int cur;
		// 遍历阶段
		for (int i = 0; i < n; i++) {
			// i -> arr[i]
			while (r > 0 && arr[stack[r - 1]] >= arr[i]) {
				cur = stack[--r];
				// cur当前弹出的位置，左边最近且小
				ans[cur][0] = r > 0 ? stack[r - 1] : -1;
				ans[cur][1] = i;
			}
			stack[r++] = i;
		}
		// 清算阶段。
		while (r > 0) {
			cur = stack[--r];
			ans[cur][0] = r > 0 ? stack[r - 1] : -1;
			ans[cur][1] = -1;
		}
		// 修正阶段
		// 左侧的答案不需要修正一定是正确的，只有右侧答案需要修正
		// 从右往左修正，n-1位置的右侧答案一定是-1，不需要修正
		for (int i = n - 2; i >= 0; i--) {
			if (ans[i][1] != -1 && arr[ans[i][1]] == arr[i]) {
				ans[i][1] = ans[ans[i][1]][1];
			}
		}
	}

}
```

**从右到左遍历**

栈中记录答案的候选项（下一个更大或者更小值），遍历元素是待求解答案元素。



