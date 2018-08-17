# 深/浅拷贝

这个话题，这个[帖子](https://www.cnblogs.com/mengdd/archive/2013/02/20/2917971.html)讲的很好

**浅拷贝**（浅复制、浅克隆）：被复制对象的所有变量都含有与原来的对象**相同的值**，而所有的对其他对象的引用**仍然指向原来的对象**。

　　换言之，浅拷贝仅仅复制所考虑的对象，而不复制它所引用的对象。

**深拷贝**（深复制、深克隆）：被复制对象的所有变量都含有与原来的对象相同的值，除去那些引用其他对象的变量。

**那些引用其他对象的变量将指向被复制过的新对象，而不再是原有的那些被引用的对象。**

　　换言之，深拷贝把要复制的对象所引用的对象都复制了一遍。



接下来的这两道题目都是深拷贝（deep copy）的实例——普通的copy（浅拷贝），所有其他对象的引用仍然指向原来的元素，所以需要重新生成实例，用于返回。

通常使用HashMap，一是方便查找，二也是可以在map当中直接生成新的实例，用于拷贝后的赋值。

## [133. Clone Graph](https://leetcode.com/problems/clone-graph/description/)

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
    public UndirectedGraphNode cloneGraph(UndirectedGraphNode root) {
        if(root==null) return null;
        Queue<UndirectedGraphNode> q = new LinkedList<>();
        q.offer(root);
        
        Map<UndirectedGraphNode, UndirectedGraphNode> map = new HashMap<>();
        map.put(root, new UndirectedGraphNode(root.label));
        
        while(!q.isEmpty()){
            UndirectedGraphNode node = q.poll();
            
            for(UndirectedGraphNode n:node.neighbors){
                if(!map.containsKey(n)){
                    map.put(n, new UndirectedGraphNode(n.label));
                    q.add(n);
                }
                
                map.get(node).neighbors.add(map.get(n));
            }
        }
        return map.get(root);
    }
}
```

这道题的DFS写法效率更高

注意，把全局变量放在外面，然后就可以进行自己的对自身的递归调用

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
    Map<Integer, UndirectedGraphNode> map = new HashMap<>();
    public UndirectedGraphNode cloneGraph(UndirectedGraphNode root) {
        if(root==null) return null;
        
        if(map.containsKey(root.label))
            return map.get(root.label);
        UndirectedGraphNode clone = new UndirectedGraphNode(root.label);
        map.put(root.label, clone);
        for(UndirectedGraphNode n:root.neighbors){
            clone.neighbors.add(cloneGraph(n));
        }
        
        return clone;
    }
}
```

## [138. Copy List with Random Pointer](https://leetcode.com/problems/copy-list-with-random-pointer/description/)

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
        if(head==null) return null;
        RandomListNode cur = head;
        Map<RandomListNode, RandomListNode> map = new HashMap<>();
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

