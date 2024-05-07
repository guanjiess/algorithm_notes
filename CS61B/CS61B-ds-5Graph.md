### Graph

A graph consists of:

- A set of nodes (or vertices)
- A set of zero of more edges, each of which connects two nodes.

以及，注意图具有有向、无向的区别.

### Graph problems   

> - **s-t Path**: Is there a path between vertices s and t?
> - **Connectivity**: Is the graph connected, i.e. is there a path between all vertices?
> - **Biconnectivity**: Is there a vertex whose removal disconnects the graph?
> - **Shortest s-t Path**: What is the shortest path between vertices s and t?
> - **Cycle Detection**: Does the graph contain any cycles?
> - **Euler Tour**: Is there a cycle that uses every edge exactly once?
> - **Hamilton Tour**: Is there a cycle that uses every vertex exactly once?
> - **Planarity**: Can you draw the graph on paper with no crossing edges?
> - **Isomorphism**: Are two graphs isomorphic (the same graph in disguise)?



### Depth First Traversal

The purpose of the algorithm is to mark each vertex as visited while avoiding cycles. 

https://www.programiz.com/dsa/graph-dfs

==图的深度优先遍历==

重要的点

- 图的点权、边权是进行遍历时最主要的参考指标，解图遍历问题时先把整体的拓扑图画出来，随后建立起来一个邻接矩阵和遍历标识向量。
- 进行DFS遍历前，需要明确那些参量需要在遍历时进行修正，随后进行相应的传参。



**细节**

- 当某一个边已经经过访问后，为了避免其被重复遍历，可以在邻接矩阵中删除这条对应的边。
- 理解题意最快速有效的方法，就是把题目涉及的概念一个个用代码写下来。

- 为了方便DEBUG，可以写一个打印图结构的函数和几个简单的测试用例。







### Breadth First Traversal

https://www.programiz.com/dsa/graph-bfs

==图的广度优先遍历==

- 用队列实现，比DFS容易很多。





### Implementation

There are two common implementations for graphs

- An adjacency matrix  邻接矩阵

  represents a graph G of N nodes and E edges using an NxN boolean matrix A, where A[i][j] is true if (i,j) is an edge in G.

- An *adjacency list*  邻接表

  represents a graph G of N nodes and E edges as an array A of size N, where A[i] is a pointer to a list of vertices that are successors to vertex i. 有点类似于hash table

数据结构的选择上,使用最多的是adjacentList.

> 两种数据结构的使用会依赖于具体的应用场景和需求。没有一个通用的答案，哪个更广泛使用，因为选择取决于问题的性质和算法的要求。
>
> 以下是一些考虑因素：
>
> 1. **空间复杂度**：
>    - 邻接表通常在稀疏图中更节省空间，因为它只存储实际存在的边。
>    - 邻接矩阵在稠密图中可能更高效，因为它存储所有可能的边，但会浪费空间，如果大部分边都是不存在的。
> 2. **时间复杂度**：
>    - 邻接表通常需要更多时间来查找特定的边或确定一个节点的邻居，因为它需要遍历链表或其他数据结构来查找。
>    - 邻接矩阵通常具有更好的时间复杂度，因为它可以通过索引直接访问边的信息。
> 3. **特定操作**：
>    - 如果需要频繁地添加或删除边，邻接表可能更适合，因为它更灵活。
>    - 如果需要经常查询边的存在性，邻接矩阵可能更合适，因为它执行这些操作的时间复杂度较低。
> 4. **存储方式**：
>    - 邻接表使用链表或数组来表示图结构，这使得它在实现上相对容易。
>    - 邻接矩阵使用二维数组，通常需要更多的内存来表示相同的图。
> 5. **图的类型**：
>    - 对于稀疏图，如社交网络图，交通网络图等，邻接表通常更适用。
>    - 对于密集图，如图像处理中的像素连接图，邻接矩阵可能更合适。
>
> 总之，哪种数据结构更广泛使用取决于具体应用场景和算法需求。在实际开发中，你需要考虑你的数据集、算法的性能要求以及内存限制等因素来选择适当的数据结构。