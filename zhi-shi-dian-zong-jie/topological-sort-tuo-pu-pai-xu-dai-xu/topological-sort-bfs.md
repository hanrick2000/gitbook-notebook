# Topological sort, BFS

无论是directed 还是directed graph，BFS方法的核心都是“indegree”，处理的顺序也就是从indegree最小的开始。

 因此在循环最开始，我们可以把所有 indegree = 0 \(也即不在 hashmap 中\) 的节点加到 list 中，也作为 BFS 的起点。

在 BFS 过程中，我们依次取出队列里取出节点 node 的 neighbors， next，并且对 next 对应的 indegree -1，代表其 parent node 已经被处理完，有效 indegree 减少。

**从 Course Schedule 的 BFS 做法学到的经验是，如果图中有环，会有环上的 node 永远无法减到 indegree = 0 的情况，导致一些 node 没有被访问。**

**这也是当初面试被问到的为什么 JVM 不只靠 reference counting 来做 GC，就是因为有环的时候，indegree 不会减到 0.**

 [127. Topological Sorting](https://www.lintcode.com/problem/topological-sorting/description)

```java
/**
 * Definition for Directed graph.
 * class DirectedGraphNode {
 *     int label;
 *     ArrayList<DirectedGraphNode> neighbors;
 *     DirectedGraphNode(int x) { label = x; neighbors = new ArrayList<DirectedGraphNode>(); }
 * };
 */

public class Solution {
    /*
     * @param graph: A list of Directed graph node
     * @return: Any topological order for the given graph.
     */
    public ArrayList<DirectedGraphNode> topSort(ArrayList<DirectedGraphNode> graph) {
        // write your code here
        
        ArrayList<DirectedGraphNode> res = new ArrayList<>();
        
        int size = graph.size();
        
        // ArrayList[] g = new ArrayList[size];
        int[] degree = new int[size];
        
        for(DirectedGraphNode node : graph){
            for(DirectedGraphNode nei : node.neighbors){
                // g[node.neighbors.get(i).label].add(node.label);
                degree[nei.label]++;
            }
        }
        Queue<DirectedGraphNode> q = new LinkedList<>();
        for(int i=0;i<size;i++){
            if(degree[i]==0) q.offer(graph.get(i));
        }
        
        while(!q.isEmpty()){
            DirectedGraphNode temp = (DirectedGraphNode)q.poll();
            res.add(temp);
            for(DirectedGraphNode n : temp.neighbors){
                degree[n.label]--;
                if(degree[n.label]==0){
                    q.offer(n);
                }
            }
            
        }
        return res;
    }
}
```

