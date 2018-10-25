# Undirected Graph, BFS

#### directed graph BFS 里靠 indegree = 0 判断加入队列； <a id="directed-graph-bfs-&#x91CC;&#x9760;-indegree--0-&#x5224;&#x65AD;&#x52A0;&#x5165;&#x961F;&#x5217;&#xFF1B;"></a>

#### undirected graph BFS 里靠 degree = 0 判断加入队列； <a id="undirected-graph-bfs-&#x91CC;&#x9760;-degree--0-&#x5224;&#x65AD;&#x52A0;&#x5165;&#x961F;&#x5217;&#xFF1B;"></a>

通常情况下，有向图只算一次degree，而无向图算两次

### [Graph Valid Tree](https://leetcode.com/problems/graph-valid-tree/) <a id="graph-valid-tree"></a>

这道题目BFS与DFS都可以做

题目要求判断一个graph是否为tree，其实就是两个判断条件：1. 图中不能有环， 2. 不能有孤立的点。

### 判断一个图里面是否有环可以有两种方法：

#### 方法1：

1. 求出图中所有顶点的度，
2. 删除图中所有度&lt;=1的顶点以及与该顶点相关的边，把与这些边相关的顶点的度减一
3. 如果还有度&lt;=1的顶点重复步骤2
4. 最后如果还存在未被删除的顶点，则表示有环；否则没有环

时间复杂度为O（E+V），其中E、V分别为图中边和顶点的数目

```java
/**
     * @param n: An integer
     * @param edges: a list of undirected edges
     * @return: true if it's a valid tree, or false
     */
    public boolean validTree(int n, int[][] edges) {
        // write your code here
        ArrayList[] g = new ArrayList[n];
        int[] degree = new int[n];
        
        for(int i=0;i<n;i++){
            g[i] = new ArrayList<>();
        }
        
        for(int[] edge : edges){
            g[edge[0]].add(edge[1]);
            g[edge[1]].add(edge[0]);
        }
        int[] status = new int[n]; 
        
        Queue<Integer> q = new LinkedList<>();
        q.offer(0);
        status[0] = 1;
        int count = 0;
        
        while(!q.isEmpty()){
            int temp = q.poll();
            count++;
            for(Object i : g[temp]){
                int ii = (int)i;
                if(status[ii]==1) return false;
                else if(status[ii]==0){
                    status[ii]=1;
                    q.offer(ii);
                }
            }
        
            status[temp] = -1;
        }
        
        return count == n;
    }
}
```

#### 方法2：

* **另一种方法是，用 int\[\] 表示每个点的状态，其中**
  * **0 代表“未访问”；**
  * **1 代表“访问中”；**
  * **2 代表“已访问”；**
* **如果在循环的任何时刻，我们试图访问一个状态为 “1” 的节点，都可以说明图中有环。**

```java
public class Solution {
    /**
     * @param n: An integer
     * @param edges: a list of undirected edges
     * @return: true if it's a valid tree, or false
     */
    public boolean validTree(int n, int[][] edges) {
        // write your code here
        if(n==1) return true;
        ArrayList[] g = new ArrayList[n];
        int[] degree = new int[n];
        
        for(int i=0;i<n;i++){
            g[i] = new ArrayList<>();
        }
        
        for(int[] edge : edges){
            g[edge[0]].add(edge[1]);
            g[edge[1]].add(edge[0]);
            degree[edge[0]]++;
            degree[edge[1]]++;
        }
        
        Queue<Integer> q = new LinkedList<>();
        for(int i=0;i<n;i++){
            if(degree[i] == 0) return false;
            else if(degree[i]==1) {
                q.offer(i);
            }
        }
        int count = q.size();
        while(!q.isEmpty()){
            int temp = q.poll();
            for(int i=0;i<g[temp].size();i++){
                int num = (int)g[temp].get(i);
                degree[num]--;
                if(degree[num]==1){
                    q.offer(num);
                    count++;
                }
            }
        }
        return count == n;
    }
}
```

