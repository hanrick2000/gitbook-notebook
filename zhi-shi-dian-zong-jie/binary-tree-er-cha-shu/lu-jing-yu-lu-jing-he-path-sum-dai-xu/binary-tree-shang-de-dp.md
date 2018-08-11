# Binary Tree上的DP

## [337. House Robber III](https://leetcode.com/problems/house-robber-iii/description/)

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
    public int rob(TreeNode root) {
        int[] res = helper(root);        
        return Math.max(res[0], res[1]);
    }
    
    private int[] helper(TreeNode node){
        if(node==null)
            return new int[2];
        int[] res = new int[2];
        int[] left = helper(node.left);
        int[] right = helper(node.right);
        res[0] = node.val+left[1]+right[1];
        res[1] = Math.max(left[0],left[1])+Math.max(right[0], right[1]);
        return res;
    }
}
```

