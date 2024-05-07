## 应用场景



## 基本操作





## 适合谁

1. 可以把原问题变为和原问题相似的、规模更小的子问题，可以用递归求解。
2. 问题存在明显的嵌套结构，当前一级问题的求解需要下一级问题的结果，则用递归。



## 怎么用、怎么理解

1. 从整体上思考问题，不要一开始陷入细节，思考整棵树和左右子树的关系。
2. 确定边界条件。
3. 确定单层的执行逻辑，也就是怎么拆分子问题、拆分多少个子问题。
4. 一般而言有两种递归实现视角，自底向上、自顶向下。
5. 递归的底层实现是栈。



##递

从根节点一级一级到叶子节点的过程

##归

从叶子节点一级一级回溯到根节点的过程

##二叉树

1. 最大深度 https://leetcode.cn/problems/maximum-depth-of-binary-tree/description/

2. 最小深度 https://leetcode.cn/problems/minimum-depth-of-binary-tree/

   有些坑需要注意，一些子问题如果考虑不周，很容易写成这种==错误形式==

   ```C++
   int minDepth(TreeNode* root) {
           if(root == nullptr) return 0;
           return 1 + min(minDepth(root->left), minDepth(root->right));
    }
   ```

    最小深度是**从根节点到最近叶子节点**的最短路径上的节点数量，如果一个子树只有左子树而没有右子树，那么计算最小深度时就会直接忽略左子树，选为0，这不符合对于子树深度的定义。

   所以需要把问题拆分为如下三个子问题。

   - 如果没有左子树
   - 如果没有右子树
   - 左右子树都存在

3. 路径总和  https://leetcode.cn/problems/path-sum/ 

4. 根节点到叶子节点总和 https://leetcode.cn/problems/sum-root-to-leaf-numbers

   - 回溯
   - 入参传入sum，到达叶子节点时求和

5. 二叉树所有路径 https://leetcode.cn/problems/binary-tree-paths/ 

6. 二叉树中的好节点数目 https://leetcode.cn/problems/count-good-nodes-in-binary-tree

7. **判断平衡二叉树**： https://leetcode.cn/problems/balanced-binary-tree/solution/ru-he-ling-huo-yun-yong-di-gui-lai-kan-s-c3wj/ 

8. 二叉树的右视图：https://leetcode.cn/problems/binary-tree-right-side-view/description/

9. 翻转二叉树： https://leetcode.cn/problems/invert-binary-tree/ 

10. 最大差值： https://leetcode.cn/problems/maximum-difference-between-node-and-ancestor/

11. **删除不足节点**：https://leetcode.cn/problems/insufficient-nodes-in-root-to-leaf-paths



## 二叉树遍历

1、由前序、中序序列构造二叉树结构

https://leetcode.cn/problems/construct-binary-tree-from-preorder-and-inorder-traversal/

```C++
TreeNode* build(vector<int>& preorder, vector<int>& inorder, int l_s, int l_e, int i_s, int i_e){
        //边界没搞太明白
        if(l_s > l_e){
            return nullptr;
        }
        TreeNode* root = new TreeNode(preorder[l_s]);
        int in_root = index[preorder[l_s]];
        int l_len = in_root - i_s;
        root->left = build(preorder, inorder, l_s+1, l_s + l_len, i_s, in_root-1);
        root->right = build(preorder, inorder, l_s + l_len + 1, l_e, in_root+1, i_e);
        return root;
}
```

2、由中序、后序构造二叉树

 https://leetcode.cn/problems/construct-binary-tree-from-inorder-and-postorder-traversal/ 

3、由前序、后序构造二叉树

 https://leetcode.cn/problems/construct-binary-tree-from-preorder-and-postorder-traversal/ 

4、倒向遍历父节点，并求最大深度

https://leetcode.cn/problems/amount-of-time-for-binary-tree-to-be-infected

5、层序遍历模板

```C++
class Solution {
public:
    vector<vector<int>> levelOrder(TreeNode* root){
        if(root == nullptr) return {};
        vector<vector<int>> ans;
        queue<TreeNode*> q;
        q.push(root);
        
        while(!q.empty()){
            vector<int> vals;
            int size = q.size();
            for(int i = 0; i < size; i++){
                auto node = q.front();
                q.pop();
                vals.push_back(node->val);
                if(node->left) q.push(node->left);
                if(node->right) q.push(node->right);
            }
            ans.emplace_back(vals);
        }
        
        return ans;
    } 
};
```







## 二叉搜索树

1. 判断二叉搜索树： https://leetcode.cn/problems/validate-binary-search-tree/ 
   - 前序：传入二叉搜索树的范围，当前节点需要满足的范围left和right。首先判断当前节点需要满足 left < x < right。左子树的值都小于x，需要更新左边界为x；右子树的值都大于x，需要更新右边界为x。随后递归判断左右子树。
   
   - 中序：中序遍历的顺序为  左子树->根->右子树，结合二叉搜索树的性质，其中序遍历得到的子序列一定是单调增的，可以运用这个方式判断其是否为二叉搜索树。过程可拆分为三个子问题，1判断左子树是否为BST、2判断根节点是否小于前驱节点、3判断右子树是否为BST。
   
     **注意**，在中序遍历过程中，实质性的操作都发生在对根节点的操作上。比如左子树会一口气递归到空节点，随后再返回到根节点，所以判断操作、保留前驱节点操作都在中间位置进行，左右子树递归即可。
   
   - 后序：把节点值的范围自底向上传。感觉比较复杂，需要一点时间消化。
   
2. BST的范围和：https://leetcode.cn/problems/range-sum-of-bst/ 

   - 观察每个节点和low  high之间的关系，据此可以分为三个子问题
   - 1 x大于high
   - 2 x小于low
   - 3 x在 low和high之间

3. 二叉搜索树的==最近节点查询==：https://leetcode.cn/problems/closest-nodes-queries-in-a-binary-search-tree/

4.  二叉搜索树中的众数  https://leetcode.cn/problems/find-mode-in-binary-search-tree/ 

5. 二叉搜索树的**最大键值和**（**后序遍历**）：https://leetcode.cn/problems/maximum-sum-bst-in-binary-tree/



##平衡二叉树





##红黑树





## 小tricks

判断叶子

```c++
if(!root->left && !root->right)
if (root->left == root->right)
```

判断左叶子

```c++
if(root->left && !root->left->left && !root->left->right)
```

判断右叶子

```c++
if(root->right && !root->right->left && !root->right->right)
```

## 