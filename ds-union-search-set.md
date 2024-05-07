## 应用场景







##并查集

Union Search Set

题单

https://github.com/itcharge/LeetCode-Py/blob/main/Contents/07.Tree/05.Union-Find/02.Union-Find-List.md



### 题目

1、等式的可满足性：https://leetcode.cn/problems/satisfiability-of-equality-equations/

思路：原等式中只有等于、不等于两种情况，可以用并查集解决，把相等的添加到一个连通块中，再去遍历不等式的情况，如果不等式两侧的元素存在于之前的相同集合中，说明等式不满足。

2、省份数量：https://leetcode.cn/problems/number-of-provinces/description/

思路：典型的并查集模板题，遍历每一条边，把每条边的两端节点添加到一个连通块中，最后统计根节点的数量。

3、冗余链接：https://leetcode.cn/problems/redundant-connection/description/

思路：该题目只有一个环，当添加的某天边同属于一个集合时，说明加入该边后成环，需要返回这条边。

- 从前向后遍历每条边，如果两个节点不在同一集合，那么加入统一集合（union操作）
- 如果边的两端同属于一个集合，则找到答案，返回该边

4、连通网络的操作次数：https://leetcode.cn/problems/number-of-operations-to-make-network-connected/

思路

1. n台计算机需要至少n-1根线才能连接，小于n-1个线缆则无法连接
2. 遍历网线连接数组，把相连的节点a和b添加到一个集合中
   - 如果ab已经在一个集合中，则该线多余，多余电线+1
   - 如果ab不在一个集合中，则合并两条线，ab之间不再需要其他连接，所需的电线总数减1
   - 判断多余的电线数是否大于等于需要的电线数，如果满足，返回所需要的电线数

5、**情侣牵手**：https://leetcode.cn/problems/couples-holding-hands/description/，这篇题解很好

https://leetcode.cn/problems/couples-holding-hands/solutions/21437/bing-cha-ji-union-find-by-shty/

6、除法求值：https://leetcode.cn/problems/evaluate-division。。还挺难的，合并集合的时候，怎么更新相较于根节点的值那一块没摸清楚，但是大致的思路是有的。





###闭环

###模板

C++

```C++

//递归形式路径压缩
int find(vector<int>& father, int x){
	if(x != father[x]) father[x] = find(father, fa[x]);
    return fa[x];
}
//递推形式路径压缩，推荐用这个
int find2(int x){
        int root = x;
        while(father[root] != root){
            root = father[root];
        }
        while(x != root){
            int original_father = father[x];
            father[x] = root;
            x = original_father;
        }        
        return root;
}

void union(vector<int>& father, int a, int b){
    int root_a = find(father, a);
    int root_b = find(father, b);
    if(root_a != root_b){
        father[root_a] = root_b;
    }
}
```

python3

```python
class UnionFind:
    def __init__(self, n):
        self.father = [i for i in range(n+1)]
    
    # x是当前节点
    def find(self, x):
        while self.father[x] != x:
            self.father[x] = self.father[self.father[x]]
            x = self.father[x]
        return x
    
    def union(self, x, y):
        root_x = self.find(x)
        root_y = self.find(y)
        if root_x == root_y:
            return False
        self.father[root_x] = root_y
        return True

    def is_connected(self, x, y):
        return self.find(x) == self.find(y)
```



==带权重的并查集==