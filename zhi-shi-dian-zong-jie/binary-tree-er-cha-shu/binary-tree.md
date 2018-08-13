---
description: Level order的写法其实就两种情况。但是我发现其实运用到的场景还挺多的
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

顺便提一下，对于collection类（set, list, stack）都可以用Collection函数去做操作，比如反向：**Collections.reverse\(···\)**

## [Populating Next Right Pointers in Each Node](https://leetcode.com/problems/populating-next-right-pointers-in-each-node/)

 不过这题规定不许手动建任何额外空间（不许存level或者用迭代写，虽然递归也用空间），但是题目的定义里面。递归不是额外的空间。

 考虑到这题给定 perfect tree，不需要考虑中间有拐弯或者空位的问题，递归解决的思路就是用一个 helper 函数传入当前的两个节点（为上一个节点的 left/right child）

* 连接左右
* 迭代依次连接左节点右侧的 path 与 右节点左侧的 path
* 递归下一层



## [101. Symmetric Tree](https://leetcode.com/problems/symmetric-tree/description/)

这道题目其实不是用level order来做的。

但是我第一次拿到这个题目的时候，就想从看这个tree的每一个level是否是对称的（即层次遍历后，得到的是否是回文来入手）

但是这个题目与上面的不一样：它不是完美二叉树，**从而导致很多奇怪的拐角**。所以它并不适合用层次遍历来做

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
public class Solution {

    public static boolean isSymmetric(TreeNode root) {
        if(root==null||(root.left==null&&root.right==null)) return true;
        
        Queue<TreeNode> q = new LinkedList<>();
        q.offer(root);
        q.offer(root);
        while(q.size()>1){
            TreeNode left = q.poll();
            TreeNode right = q.poll();
            
            if(left.val!=right.val) return false;
            if(left.left!=null&&right.right!=null){
                q.offer(left.left);
                q.offer(right.right);
            }else if(left.left!=null||right.right!=null){
                return false;
            }
            if(left.right!=null&&right.left!=null){
                q.offer(left.right);
                q.offer(right.left);
            }else if(left.right!=null||right.left!=null){
                return false;
            }
            
        }
        return true;
    }
}
```



