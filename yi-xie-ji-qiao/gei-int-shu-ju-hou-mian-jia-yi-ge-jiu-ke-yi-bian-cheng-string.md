# 给int数据后面加一个“”就可以变成String

## [606. Construct String from Binary Tree](https://leetcode.com/problems/construct-string-from-binary-tree/description/)

做的时候有点卡在如何构造String类型上面，发现其实直接加一个`+""` 就好了

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
    public String tree2str(TreeNode t) {
        if(t==null) return "";
        
        String res = t.val+"";
        
        String left = tree2str(t.left);
        String right = tree2str(t.right);
        
        if(left==""&&right=="") return res;
        if(left=="") return res+"()"+"("+right+")";
        if(right=="") return res+"("+left+")";
        return res+"("+left+")"+"("+right+")";
    }
}
```

