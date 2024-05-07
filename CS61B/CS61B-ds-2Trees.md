### Tree

**Properties of trees**

> Trees are composed of:
>
> - nodes
> - edges that connect those nodes.
>   - **Constraint**: there is only one path between any two nodes.
> - **root** node which is a node that has no parents. 
> -  **leaves**, which are nodes with no children 



### **Binary Tree**

> in addition to the above requirements, also hold the binary property constraint. That is, each node has either 0, 1, or 2 children. 
>

### **Binary Search Tree**

> **Binary Search Trees**: in addition to all of the above requirements, also hold the property that For every node X in the tree:
>
> - Every key in the left subtree is less than X’s key.
> - Every key in the right subtree is greater than X’s key. **Remember this property!!

**operations**

1. search：搜索时间复杂度正比于log(N)


2. insert 

3. delete: 3 cases, no children,  one child, two children.



### Balanced Search Trees

> 在二叉搜索树（Binary Search Tree，BST）中，"depth"（深度）和"height"（高度）是两个重要的概念，它们用于描述树的结构和节点的位置：
>
> 1. **深度（Depth）：**
>    - 对于BST中的任何节点N，其深度表示从根节点到节点N的路径上的边数（或节点数）。也就是说，节点N的深度表示了它在树中的层级位置。
>    - 根节点的深度通常为0，而其子节点的深度为1，依此类推。深度值是从上往下递增的。
>    - 深度用于描述节点在树中的相对位置，例如，如果你想知道某个节点距离根节点有多远，就可以使用深度来表示。
> 2. **高度（Height）：**
>    - 对于BST中的任何节点N，其高度表示从节点N到其子树中的最远叶子节点的路径上的边数（或节点数）。也就是说，节点N的高度表示了以节点N为根的子树的最大深度。
>    - 一棵空树的高度通常定义为-1，而一棵只有根节点的树的高度为0。高度值是从下往上递增的。
>    - 高度用于描述树本身的结构，以及树的平衡性。一棵平衡的BST的高度通常较小，使得树的操作（如搜索、插入和删除）更加高效。
>
> 需要注意的是，BST的性能与树的高度密切相关。在平衡的BST中，树的高度较小，操作的平均时间复杂度较低，而在不平衡的BST中，树的高度可能会增加，导致操作的性能下降。因此，维护BST的平衡是非常重要的，以确保高效的操作。有各种平衡二叉搜索树（如AVL树、红黑树等）设计旨在保持树的平衡，从而保证高效的性能

**例如**

```markdown
        8
       / \
      3   10
     / \    \
    1   6    14
       / \   / \
      4   7  13  15
```

> - 根节点的深度为0。
> - 节点3的深度为1，节点10的深度为1。
> - 节点1的深度为2，节点6的深度为2，节点14的深度为2。
> - 节点4的深度为3，节点7的深度为3，节点13的深度为3，节点15的深度为3。
> - 树的高度是3，因为从根节点到最深的叶子节点（如节点4、7、13、15）的最长路径有3条边。

==算法笔记9.4讲解相当详细，而且有配套的代码实现和图解==







### B-Trees

> B树（B-tree，也称为Balanced Tree或多路搜索树）是一种自平衡的树状数据结构，通常用于数据库和文件系统等存储和检索大量数据的应用中。B树被设计成能够高效地支持数据的插入、删除和查找操作，并且在树的平衡性方面具有特定的性质，以确保树的高度保持相对较小，从而提高了检索操作的性能。
>
> B树的特点包括：
>
> 1. 自平衡性：B树会在插入和删除操作时自动调整树的结构，以保持树的平衡性。这意味着每个节点都会尽量包含相等数量的子节点，从而保持树的高度相对较小，提高了数据检索的效率。
> 2. 多路性：B树的每个节点可以包含多个子节点。这是与二叉搜索树不同的地方，它只有两个子节点。多路性使得B树能够容纳更多的数据，并且减少了树的深度。
> 3. 顺序性：B树节点内的数据通常是按照顺序排列的，这使得范围查询变得更加高效。在数据库中，这对于按照索引范围检索数据非常有用。
> 4. 分支因子：B树的节点可以包含的子节点数量受到一个称为“分支因子”（或阶数）的限制。分支因子决定了树的平衡性和性能。较大的分支因子可以减少树的高度，但也增加了每个节点的大小。

**B树的一些性质**

> A B-tree has the following helpful invariants:
>
> - All leaves must be the same distance from the source.
> - A non-leaf node with **k** items must have exactly ***k*+1** children.
> -  **always remain balanced**

==图示==

https://www.cs.usfca.edu/~galles/visualization/BTree.html

### Rotating Trees



### Red-Black Trees



### 遍历

1. 根据访问根节点的顺序，可以把二叉树的遍历方式分为，前序、中序、后序

2. > 前序遍历
   >
   > 1. 访问根节点。
   >
   > 2. 前序遍历左子树。
   >
   > 3. 前序遍历右子树。
   >
   > 4. ```java
   >   class TreeNode {
   >   int val;
   >   TreeNode left;
   >   TreeNode right;
   >   ```
   > ```
   > 
   > ​```java
   > public void preorderTraversal(TreeNode root) {
   > if (root == null) {
   >   return;
   > }
   > System.out.print(root.val + " "); // 访问根节点
   > preorderTraversal(root.left);    // 前序遍历左子树
   > preorderTraversal(root.right);   // 前序遍历右子树
   > }
   > ```
   >
3. > 1. 中序遍历左子树。
   > 2. 访问根节点。
   > 3. 中序遍历右子树。
   >
   > 
   >
   > ```java
   > public void inorderTraversal(TreeNode root) {
   >     if (root == null) {
   >         return;
   >     }
   >     inorderTraversal(root.left);     // 中序遍历左子树
   >     System.out.print(root.val + " "); // 访问根节点
   >     inorderTraversal(root.right);    // 中序遍历右子树
   > }
   > 
   > ```
   >
   > 

4. > 1. 后序遍历左子树。
   > 2. 后序遍历右子树。
   > 3. 访问根节点。
   >
   > ```java
   > public void postorderTraversal(TreeNode root) {
   >     if (root == null) {
   >         return;
   >     }
   >     postorderTraversal(root.left);    // 后序遍历左子树
   >     postorderTraversal(root.right);   // 后序遍历右子树
   >     System.out.print(root.val + " "); // 访问根节点
   > }
   > 
   > ```



### 递归

1. 采用一种分而治之的思想，把复杂的问题转为更简单的子问题。子问题和原问题有一样的结构。
2. 需要确定几个基本问题：终止条件（base case）,子问题的逻辑，返回值。--->信仰之跃
3. 计算机内部实现的底层原理是栈，即为LIFO(Last in first out)
4. CS61A有不少相关内容。
5. https://www.geeksforgeeks.org/tree-traversals-inorder-preorder-and-postorder/











