##DFS

DFS，depth first search，深度优先搜索：Wiki版本 https://en.wikipedia.org/wiki/Depth-first_search

> DFS is an algorithm for traversing a tree or a graph data structures. The algorithm starts at the root bide and explores as far as possible along each branch before backtracking.

我的理解：在给定限制条件下，完整地走完所有路径的搜索方法。一条道走到黑。



例1：一共有n个货物，每个货物都有对应的价值、重量，在背包里的容量（容量为c）有限的前提下，怎么让包里装的货物的价值最大？

如果用DFS的思考：每个货物只有放或者不放如背包两种选择（一种二叉树），那么物品的组合就有2的n次方种可能性，也就是2^n条不同的路径，DFS的做法就是用某种方式把这些路径全部走一遍，计算得出自己最需要的结果。（所以DFS的实现和二叉树、递归是高度相关的）

<img src="E:\master2\coding_notes\DSA\bfs_dfs.assets\Depth-First-Search.gif" style="zoom:50%;" />

例2：给定N个整数，从中选择K个数字，使得他们的和恰好为一个约定好的整数X；如果有多个方案，选择平方和最大的方案。

**分析**

- 从N个数字中选取K个数字，显然每个数字只有选或不选两种情况，可以用DFS搜索。
- 边界条件1：当前选择数字达到K，达到约束上限
- 边界条件2：突破容量上限，即超过了N个整数；超过了K个整数；当前数字的和已经超过了X；
- 注意：DFS对应的分叉指某个具体的选项，做或不做。



## BFS

BFS，breadth first search，广度优先搜索：

我的理解：广度优先搜索的主要思想是逐层地寻找目标节点，对每一层的数据进行匹配，当本层数据全部无效后，再去查询下一层的数据。可以理解为，迷宫里有很多个岔路口，我把每个岔路口都往前走一步，记录下来新的岔路口以后，再把这些新的岔路口顺着往前走一步，直到最后走出去。

一般BFS的实现思路：一般用队列实现，从根节点开始逐级迭代，每一轮迭代时：首先取出队列的头节点--->对头节点的邻接节点分别进行匹配，如果满足条件，则记录节点所在层级并入队--->对同层其他节点的邻接点进行匹配-

-->循环，直到得到最终结果。



###BFS-队列

以二叉树的层序遍历为例.

C++

```C++

//Definition for a binary tree node.
struct TreeNode {
    int val;
    TreeNode *left;
    TreeNode *right;
    TreeNode() : val(0), left(nullptr), right(nullptr) {}
    TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
    TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right) {}
};

class Solution {
public:
    vector<vector<int>> zigzagLevelOrder(TreeNode* root) {
        vector<vector<int>> ans;
        if(root == nullptr){
            return ans;
        }
        queue<TreeNode*> q;
        q.push(root);
        bool even = false;
        while(!q.empty()){
            vector<int> vals;
            int size = q.size();
            for(int i = 0; i < size; i++){

                TreeNode* node = q.front();
                q.pop();
                vals.push_back(node->val);

                if(node->left) q.push(node->left);
                if(node->right) q.push(node->right);
                
            }
            if(even){
                int i = 0, j = vals.size()-1;
                for(; i < j; i++, j--) {
                    swap(vals[i], vals[j]);
                }
                // ranges::reverse(vals)
            }
            ans.push_back(vals);
            even = !even;
        }
        return ans;
    }
};
```

python

```python
class TreeNode:
    def __init__(self, val=0, left=None, right=None):
         self.val = val
         self.left = left
         self.right = right
class Solution:
    def zigzagLevelOrder(self, root: Optional[TreeNode]) -> List[List[int]]:
        # 总是忘了边界
        if root is None:
            return []
        ans = []
        q = deque([root])
        even = False
        while q:
            size = len(q)
            vals = []
            for _ in range(size):
                node = q.popleft()
                vals.append(node.val)
                if node.left:
                    q.append(node.left)
                if node.right:
                    q.append(node.right)
            vals = vals[::-1] if even else vals
            ans.append(vals)
            even = not even
        return ans
```





###BFS-两个数组

以二叉树的层序遍历为例.

c++

```C++
    vector<vector<int>> levelOrder(TreeNode* root){
        if(root == nullptr){
            return {};
        }
        vector<vector<int>> ans;
        vector<TreeNode*> cur;
        cur.push_back(root);

        // 只有最后一层元素会为空
        while(!cur.empty()){
            vector<TreeNode*> nxt;
            vector<int> vals;
            for(TreeNode* node : cur){
                //对当前层元素做具体操作
                vals.push_back(node->val);
                if(node->left){
                    nxt.push_back(node->left);
                }
                if(node->right){
                    nxt.push_back(node->right);
                }
            }
            ans.push_back(vals);
            // 替换cur为下一层
            cur = nxt;
        }

        return ans;
    } 
```



python

```python
class Solution:
    def levelOrder(self, root: Optional[TreeNode]) -> List[List[int]]:
        if root is None:
            return []
        cur = [root]
        ans = []

        # cur为当前层级结点
        # nxt存储下一层结点
        while cur:
            nxt = []
            vals = []
            for node in cur:
                vals.append(node.val)
                if node.left:
                    nxt.append(node.left)
                if node.right:
                    nxt.append(node.right)
            cur = nxt
            ans.append(vals)
        return ans
```

###模板题

102. 二叉树的层序遍历 https://leetcode.cn/problems/binary-tree-level-order-traversal/solution/bfs-wei-shi-yao-yao-yong-dui-lie-yi-ge-s-xlpz/ 

103. 二叉树的锯齿形层序遍历 https://leetcode.cn/problems/binary-tree-zigzag-level-order-traversal/solution/bfs-wei-shi-yao-yao-yong-dui-lie-yi-ge-s-xlv3/ 
104. 找树左下角的值（amazing，从右到左遍历，很灵活） https://leetcode.cn/problems/find-bottom-left-tree-value/solution/bfs-wei-shi-yao-yao-yong-dui-lie-yi-ge-s-f34y/ 

199. 二叉树的右视图 https://leetcode.cn/problems/binary-tree-right-side-view/ 





## 树

DFS：递归





BFS：层序遍历

- 多源
- 单源



## 网格图

DFS：类似于二叉树，实际上是一个四叉树的递归。

1、岛屿数量：https://leetcode.cn/problems/number-of-islands/

2、岛屿面积：https://leetcode.cn/problems/max-area-of-island/

3、岛屿周长：https://leetcode.cn/problems/island-perimeter/description/

注意点

1、避免遍历曾经遍历过的节点，否则会进入死循环。



## 图







## 易错

1、边界的检测问题

```C++
    	int area(vector<vector<int>>& grid, int r, int c){
        //注意边界问题，网格的边界是  0 ~ r-1，检测是否越界时应该用 r>=size，而不是 > ，否则会陷入死循环
        if(r < 0 || r >= grid.size() || c < 0 || c >= grid[0].size()){
            return 0;
        }
```



































