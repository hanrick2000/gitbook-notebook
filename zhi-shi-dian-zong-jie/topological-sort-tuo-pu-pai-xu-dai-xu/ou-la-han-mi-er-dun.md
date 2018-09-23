# 欧拉/汉密尔顿

**Greedy DFS, building the route backwards when retreating.**

欧拉回路：指能够一遍不重复的走过所有边的图（经过点次数可以不计）

哈密尔顿圈：指能狗一次不重复走过所有点的图（经过边的次数可以不计）

对于哈密尔顿圈，并没有什么有效的算法，对于欧拉回路我们可以使用基于DFS算法的思想来寻求一张图是否存在回路

当然，欧拉回路其实有一下性质：

\*如果图中奇数度的顶点个数小于等于2，则一定存在欧拉回路

\*在无向图中每个顶点的度都是偶数，则一定存在回路

\*在有向图中，每个节点的入度等于出度，则存在回路  


这题其实和我之前用 DFS 处理 topological sort 的代码非常像，主要区别在于存 graph 的方式不同，这里是一个 String 直接连着对应的 next nodes，而且形式是 min heap:

> * **原题给的是 edges，所以图是自己用 hashmap 建的。**
>   * **min heap 可以自动保证先访问 lexicographical order 较小的；**
>   * **同时 poll 出来的 node 自动删除，免去了用 List 的话要先 collections.sort 再 remove 的麻烦。**
>   * **这种以 “edge” 为重心的算法多靠 heap，比如 dijkstra.**

```java
public class Solution {
    public List<String> findItinerary(String[][] tickets) {
        LinkedList<String> list = new LinkedList<>();

        HashMap<String, PriorityQueue<String>> map = new HashMap<>();
        for(String[] ticket : tickets){
            if(!map.containsKey(ticket[0])) map.put(ticket[0], new PriorityQueue<String>());

            map.get(ticket[0]).add(ticket[1]);
        }

        dfs(list, "JFK", map);

        return new ArrayList<String>(list);
    }

    private void dfs(LinkedList<String> list, String airport, HashMap<String, PriorityQueue<String>> map){
        while(map.containsKey(airport) && !map.get(airport).isEmpty()){
            dfs(list, map.get(airport).poll(), map);
        }
        list.offerFirst(airport);
    }
}
```

