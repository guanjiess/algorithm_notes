##建图

邻接表==边权建图==

```c++
void dfs(int u,int fa){
    for(auto &[x,w]:g[u]){
        if(x==fa)continue;
        dfs(x,u);
        //代码逻辑
    }
}
int main(){
    cin>>n;
    for(int i=1;i<n;i++){
        int a,b,c;
        cin>>a>>b>>c;
        g[a].push_back({b,c});
        g[b].push_back({a,c});
    }
    return 0;
}
```

```python
from collections import defaultdict

n = int(input())
g = defaultdict(list)
f = [0] * (n + 1)

def dfs(u, fa):
    global f
    for x, w in g[u]:
        if x == fa:
            continue
        dfs(x, u)
        #代码逻辑

for _ in range(1, n):
    a, b, c = map(int, input().split())
    g[a].append((b, c))
    g[b].append((a, c))

```



邻接表==点权建图==

```c++
#include <bits/stdc++.h>
using namespace std;
const int N=1E5+10;
vector<int>g[N];  //领接表的vector写法 仅适用于点权建图
int w[N];
void dfs(int u,int fa) //如果是有向图 就不需要fa这个变量
{
    //do things
    for(int &x:g[u]) //访问u的所有节点
    {
        if(x==fa)continue; //无向边才需要这一句 保证每个节点只会被访问一次(不理解的可以直接背过)
        dfs(x,u);
        // do things
    }
}

int main(){
    int n,m;  //n个点,m条边
    cin>>n>>m;
    for(int i=0;i<m;i++){
        int a,b;
        cin>>a>>b;
        g[a].push_back(b);  //a->b建立一条边
    }
    for(int i=1;i<=n;i++)cin>>w[i];
    return 0;
}
```

```python
N = 100010
g = [[] for _ in range(N)]
w = [0] * N

def dfs(u, fa):
    # Do things
    for x in g[u]:
        if x == fa:
            continue
        dfs(x, u)
        # Do things

n, m = map(int, input().split())
for i in range(m):
    a, b = map(int, input().split())
    g[a].append(b)  # a->b建立一条边

w[1:] = map(int, input().split())

```



## 图的遍历

DFS：深度优先搜索，递归。

1、有向无环图中节点的祖先：https://leetcode.cn/problems/all-ancestors-of-a-node-in-a-directed-acyclic-graph/description/

2、课程表：https://leetcode.cn/problems/course-schedule/





BFS

1、课程表：https://leetcode.cn/problems/course-schedule/



## 有向图

1、课程表问题：https://leetcode.cn/problems/course-schedule/

https://leetcode.cn/problems/course-schedule/solutions/250377/bao-mu-shi-ti-jie-shou-ba-shou-da-tong-tuo-bu-pai-/?envType=study-plan-v2&envId=top-100-liked







## 无向图

https://leetcode.cn/problems/count-pairs-of-connectable-servers-in-a-weighted-tree-network/

https://leetcode.cn/problems/count-valid-paths-in-a-tree/

## tricks

==打印图==

