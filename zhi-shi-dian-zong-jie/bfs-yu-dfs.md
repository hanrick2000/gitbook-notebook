# BFS与DFS

## [515. Find Largest Value in Each Tree Row](https://leetcode.com/problems/find-largest-value-in-each-tree-row/description/)

我想利用这趟题目来帮助展示BFS与DFS的不同

### BFS方法

利用Queue，要注意Java使用Queue的时候要用

```java
Queue<Integer> queue = new LinkedList<>();

queue.add();
queue.poll();
```

其实这个就是层次遍历加上一些额外的比较。

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
    
    public List<Integer> largestValues(TreeNode root) {
        List<Integer> res = new ArrayList<>();
        Queue<TreeNode> queue = new LinkedList<>();
        if(root==null) return res;
        queue.add(root);
        int size = 1;
        while(size>0){
            int max = Integer.MIN_VALUE;
            for(int i=0;i<size;i++){
                TreeNode tmp = queue.poll();
                max = Math.max(max,tmp.val);
                if(tmp.left!=null) queue.add(tmp.left);
                if(tmp.right!=null) queue.add(tmp.right);
            }
            res.add(max);
            size = queue.size();
        }
        
        return res;
    }
}
```

### DFS方法

利用栈（在算法中就直接简化成了使用递归）

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
    public List<Integer> largestValues(TreeNode root) {
        List<Integer> res = new ArrayList<>();
        helper(res, root, 0);
        return res;
    }
    
    private void helper(List<Integer> list, TreeNode root, int layer){
        if(root==null)
            return;
        if(layer==list.size()){
            list.add(root.val);
        }else{
            list.set(layer, Math.max(root.val, list.get(layer)));
        }
        
        helper(list, root.left, layer+1);
        helper(list, root.right, layer+1);
    }
}
```

