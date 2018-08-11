---
description: 我关注的那个博主堆中序遍历专门写了一个专题，而且相关例题也很多，看来还挺重要的。而且我也确实遇到很多类似做法的题目，以后再遇到的时候都会整理到这里来
---

# Level Order 遍历

## [**Binary Tree Level Order Traversal**](https://leetcode.com/problems/binary-tree-level-order-traversal/)\*\*\*\*

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
    public List<List<Integer>> levelOrder(TreeNode root) {
        Queue<TreeNode> queue = new LinkedList<>();
        List<List<Integer>> res = new ArrayList<>();
        
        if(root!=null) queue.offer(root);
        while(!queue.isEmpty()){
            int size = queue.size();
            List<Integer> list = new ArrayList<>();
            for(int i=0;i<size;i++){
                TreeNode tmp = queue.poll();
                list.add(tmp.val);
                if(tmp.left!=null) queue.offer(tmp.left);
                if(tmp.right!=null) queue.offer(tmp.right);
            }
            res.add(list);
        }
        return res;
    }
}
```

同时也可以留意一下这个题目的递归的写法

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
    
    List<List<Integer>> res = new ArrayList<>();
    
    public List<List<Integer>> levelOrder(TreeNode root) {
        helper(root, 0);
        return res;
    }
    
    private void helper(TreeNode node, int level){
        if(node == null) return;
        
        if(level>=res.size())
            res.add(new ArrayList<>());
        
        res.get(level).add(node.val);
        helper(node.left, level+1);
        helper(node.right, level+1);
    }
}
```



