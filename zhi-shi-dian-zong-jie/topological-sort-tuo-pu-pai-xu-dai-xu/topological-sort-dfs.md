# Topological sort, DFS

要想得到拓扑排序的， 举例在下图中，一定会有一个belt-&gt;jacket的序列，但是这个子序列也一定会在pants-&gt;belt的后面。

因为使用DFS会首先到达最底层再递归，所以，要进行一个“插入”的动作

![](../../.gitbook/assets/image%20%281%29.png)

针对这样的需求，LinkedList有API addFirst\(\)，可以在前面插值。

[Topological sort](https://www.lintcode.com/problem/topological-sorting/description)

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
        LinkedList<DirectedGraphNode> list = new LinkedList<>();
        HashSet<DirectedGraphNode> visited = new HashSet<>();
        
        for(DirectedGraphNode node : graph){
            dfs(node, list, visited);
        }
        return new ArrayList(list);
    }
    
    private void dfs(DirectedGraphNode node, LinkedList<DirectedGraphNode> list, 
                    Set<DirectedGraphNode> visited){
                        if(visited.contains(node)) return;
                        
                        for(DirectedGraphNode neighbor : node.neighbors){
                            dfs(neighbor, list, visited);
                        }
                        
                        list.addFirst(node);
                        visited.add(node);
                    }
}
```

