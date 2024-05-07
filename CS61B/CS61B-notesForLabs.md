1. to run a Java file that is within a package, we must enter the parent directory (in our case, `lab6`) and use the fully canonical name. 

### Lab6

2023.09.15

#### 内容

- [x] 介绍了一些新的工具，通过命令行编译和执行程序、make测试（主要是用于命令行背景下的自动化测试）

  ```bash
  java c  cpears/*.java
  java cears.Main  
  make
  make check
  ```

  

- [x] 介绍了怎么用java中管理文件，和一些输入输出信息流的概念，其实我没看太明白，但是大受震撼。

```java
java.io
java.io.File
BufferedOutputStream
ObjectInputStream
```

- [x] 绝对路径、相对路径这些，直接问GPT
- [x] 写了一个有不同指令的函数，整体的流程是：

```java
//在主函数输入指令和参数
java cpears.Main [args] [parameters]
//主函数根据收到的字符串，调用相应的函数并进行传参，如
java cpears.Main dog dog1 poodle 3
//在main函数内进行调用相应的方法，典型的OOP.
CapersRepository.makeDog(name, breed, age);
```

- [x] 要熟悉一些轮子的使用，比如上面提到的输入输出信息流

#### 教训

- [x] 读文档，理解需求。
- [x] 读文档，看Hug给的建议



### Lab7

开始：2023.09.19, 10 : 30

**任务**

>  In this lab, you’ll create **BSTMap**, a BST-based implementation of the Map61B interface, which represents a basic tree-based map. You will be creating this completely from scratch, using the interface provided as your guide. 

**要求**

- Create a class BSTMap implements ==all the methods given in Map61B==, except for remove, iterator, keySet,  For these methods you should throw an `UnsupportedOperationException`. 
- Using Binary Search Tree
-  assume that generic keys `K` in `BSTMap` extend [Comparable](https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/lang/Comparable.html) 

**分析**

总的来说是一个Binary Search Tree based based BSTMap. Map 和 BST我都不懂，分解来学。

Map:  课程自定义的接口，需要完成的有`clear, containsKey, get, size, put `，可选的有`keySet, remove`

BST: Binary Search Tree，需要简单实现并且测试一下。

> https://algs4.cs.princeton.edu/32bst/BST.java.html

**内容总结**

1. put，实现元素的插入
2. get，实现指定元素的查找取值
3. contain，判断tree-map中是否包含有相应的key值

总的来说，因为二叉树本身的结构特征，相关的很多操作都可以用递归实现，这点有点接近于CS16A

**遗留问题**

- [x] printInOrder，可以采用三种遍历方案：`前序遍历、中序遍历、后序遍历`
- [ ] delete，从树中删除指定的节点。

> 从二叉搜索树 (Binary Search Tree，BST) 中删除特定的 key 通常需要遵循一些规则和操作，以确保删除后的树仍然是有效的 BST。以下是删除 BST 中特定 key 的一般步骤：
>
> 1. **定位要删除的节点**： 首先，需要找到包含要删除 key 的节点。从根节点开始，递归地或使用迭代方法沿着树的路径搜索节点，直到找到具有匹配 key 的节点，或者确定该 key 不在树中。
>
> 2. **删除节点**： 一旦找到要删除的节点，根据其子节点的情况执行不同的操作：
>
>    - 如果要删除的节点没有子节点（是叶子节点），则可以直接删除该节点，将其父节点的对应子节点设置为 null。
>    - 如果要删除的节点只有一个子节点（左子节点或右子节点），可以将其父节点的对应子节点指向要删除节点的子节点。
>    - 如果要删除的节点有两个子节点，通常需要找到其右子树中的最小节点或左子树中的最大节点来替代它。找到这个节点后，将其值复制到要删除的节点，然后递归地删除这个最小节点或最大节点。
>
> 3. **维护 BST 的性质**： 无论哪种情况，删除节点后，都必须确保 BST 的性质仍然保持不变。即左子树中的所有节点小于父节点，右子树中的所有节点大于父节点。
>
>    ```
>    class TreeNode {
>        int val;
>        TreeNode left;
>        TreeNode right;
>        TreeNode(int val) {
>            this.val = val;
>        }
>    }
>    
>    public TreeNode deleteNode(TreeNode root, int key) {
>        if (root == null) {
>            return null;
>        }
>        
>        if (key < root.val) {
>            root.left = deleteNode(root.left, key);
>        } else if (key > root.val) {
>            root.right = deleteNode(root.right, key);
>        } else {
>            // 找到要删除的节点
>            if (root.left == null) {
>                return root.right;
>            } else if (root.right == null) {
>                return root.left;
>            }
>            
>            // 有两个子节点，找到右子树中的最小节点替代
>            root.val = findMin(root.right);
>            root.right = deleteNode(root.right, root.val);
>        }
>        
>        return root;
>    }
>    
>    private int findMin(TreeNode node) {
>        while (node.left != null) {
>            node = node.left;
>        }
>        return node.val;
>    }
>    
>    ```



### lab8

hash table

























