# 深拷贝问题（图的BFS）

图的赋值其实都是reference的引用，即他是mutable的。所以在内部直接复制也无妨

### [Clone Graph](https://leetcode.com/problems/clone-graph/) <a id="clone-graph"></a>

先用一个 getNodes 函数返回所有的节点\(因为函数就给了一个\)，在返回的时候直接 new ArrayList&lt;&gt;\(set\) 调用 Collection constructor.

这样算法逻辑就分成两步：

* 过一遍所有节点，建新节点放到 HashMap 中；
* 再过一遍所有节点，这次更新每个对应节点的 neighbors.

```java
/**
 * Definition for undirected graph.
 * class UndirectedGraphNode {
 *     int label;
 *     List<UndirectedGraphNode> neighbors;
 *     UndirectedGraphNode(int x) { label = x; neighbors = new ArrayList<UndirectedGraphNode>(); }
 * };
 */
public class Solution {
    public UndirectedGraphNode cloneGraph(UndirectedGraphNode node) {
        if(node == null) return null;
        Map<UndirectedGraphNode, UndirectedGraphNode> map = new HashMap<>();
        
        Queue<UndirectedGraphNode> q = new LinkedList<>();
        Set<UndirectedGraphNode> visited = new HashSet<>();
        
        //List<UndirectedGraphNode> list = new ArrayList<>();
        q.offer(node);
        visited.add(node);
        while(!q.isEmpty()){
            UndirectedGraphNode n = q.poll();
            for(UndirectedGraphNode neighbor : n.neighbors){
                if(!visited.contains(neighbor)){
                    visited.add(neighbor);
                    q.offer(neighbor);
                }
            }
        }
        List<UndirectedGraphNode> list = new ArrayList<>(visited);
        
        for(UndirectedGraphNode l : list){
            map.put(l, new UndirectedGraphNode(l.label));
        }
        
        for(UndirectedGraphNode l : list){
            UndirectedGraphNode copy = map.get(l);
            
            for(UndirectedGraphNode nei : l.neighbors){
                copy.neighbors.add(map.get(nei));
            }
        }
        return map.get(node);
    }
}
```

### [Copy List with Random Pointer](https://leetcode.com/problems/copy-list-with-random-pointer/) <a id="copy-list-with-random-pointer"></a>

链表也是图啊，就是这么简单，轻松，愉快~~

这题还有一个比较妖孽的 follow-up，不让用 HashMap. 做法就是在每个节点后面插入新节点，最后再拆出来就行了，复杂度依然 O\(n\).

```java
/**
 * Definition for singly-linked list with a random pointer.
 * class RandomListNode {
 *     int label;
 *     RandomListNode next, random;
 *     RandomListNode(int x) { this.label = x; }
 * };
 */
public class Solution {
    public RandomListNode copyRandomList(RandomListNode head) {
        
        if(head == null) return null;
        
        Map<RandomListNode, RandomListNode> map = new HashMap<>();
        
        RandomListNode cur = head;
        
        while(cur!=null){
            map.put(cur, new RandomListNode(cur.label));
            cur = cur.next;
        }
        
        cur = head;
        while(cur!=null){
            map.get(cur).next = map.get(cur.next);
            map.get(cur).random = map.get(cur.random);
            cur = cur.next;
        }
        return map.get(head);
    }
}
```

