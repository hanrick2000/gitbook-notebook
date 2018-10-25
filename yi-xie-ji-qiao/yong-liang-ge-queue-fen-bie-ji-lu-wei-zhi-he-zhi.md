# 用两个queue，分别记录位置和值

## [Binary Tree Vertical Order Traverse](https://leetcode.com/problems/binary-tree-vertical-order-traversal/)

因为要同时记录当前点的值和当前点的层次位置，所以用了两个点queue分别记录。两个queue会同时出队与入队，所以可以保证同步。

其实想想，也可以让queue里面的值是个int或者map

```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode(int x) { val = x; }
 * }
 */
class Solution {
    public List<List<Integer>> verticalOrder(TreeNode root) {
        TreeMap<Integer, ArrayList<Integer>> map = 
            new TreeMap<>();
        List<List<Integer>> res = new ArrayList<>();
        Queue<TreeNode> q = new LinkedList<>();
        Queue<Integer> col = new LinkedList<>();
        if(root != null)
            q.offer(root);
        col.offer(0);
        while(!q.isEmpty()) {
            
            TreeNode tmp = q.poll();
            int curCol = col.poll();
            if(!map.containsKey(curCol)) {
                map.put(curCol, new ArrayList<>());
            }
            map.get(curCol).add(tmp.val);
            if(tmp.left != null) {
                q.offer(tmp.left);
                col.offer(curCol - 1);
            } 
            
            if(tmp.right != null) {
                q.offer(tmp.right);
                col.offer(curCol +1);
            } 
        }
        
        for(int index : map.keySet()) {
            res.add(map.get(index));
        }
        
        return res;
    }
}
```

