# 子树组合， BST query（待续, 还有两个会员题）

## [Unique Binary Search Trees](https://leetcode.com/problems/unique-binary-search-trees/)

这题考察的是 Binary Tree 和 BST 的结构，比较容易想到的思路是画几个 base case 出来然后开始往上加新的 node，不过容易陷进去，开始去抠 detail 去开始想如何添加新点而不违反 BST 性质。

上面那个思路只对了一半，因为 F\(n\) 是和之前的解 F\(n - a\) 有联系的，但是思考方向错了。

**正确的思路是，给定 n 个 node 之后，直接看 inorder 数组【1, 2, 3, 4 .. n-1, n】**

**那么选任意位置的元素为 root，都可以建出来一个 valid BST，左子树为 index 左边的 subarray，右子树为 index 右边的 subarray.**

**于是这就变成了一个利用递归结构的“组合”问题了，解的数量左右相乘。inorder 是 BST 的灵魂啊。**

```java
class Solution {
    public int numTrees(int n) {
        if(n<=2) return n;
        int[] dp = new int[n+1];
        
        dp[0] = 1;
        dp[1] = 1;
        
        for(int i=2;i<=n;i++){
            for(int j = 0;j<=i-1;j++){
                int left = j;
                int right = i-j-1;
                
                dp[i] += dp[left]*dp[right];
            }
        }
        
        return dp[n];
    }
}
```

### 这个地方的DP，DP\[0\]取得是1

DP中比较常见的就是确立dp\[0\]和dp\[1\]为初始状态，要注意的是不要想当然地设置dp\[0\]=0，还是要具体问题具体分析

## [Unique Binary Search Trees II](https://leetcode.com/problems/unique-binary-search-trees-ii/)

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
    
    public List<TreeNode> generateTrees(int n) {
        if(n==0) return new ArrayList<TreeNode>();
        return helper(1,n);
    }
    
    private List<TreeNode> helper(int s, int e){
        List<TreeNode> list = new ArrayList<TreeNode>();
        
        if(s>e){
            list.add(null);
            return list;
        }
        if(s==e){
            list.add(new TreeNode(s));
            return list;
        }
        
        for(int i=s;i<=e;i++){
            List<TreeNode> left = helper(s, i-1);
            List<TreeNode> right = helper(i+1, e);
            
            for(TreeNode l:left){
                for(TreeNode r:right){
                    TreeNode node = new TreeNode(i);
                    node.left = l;
                    node.right = r;
                    list.add(node);
                }
            }
        }
        return list;
    }
}
```

#### [Closest Binary Search Tree Value](https://leetcode.com/problems/closest-binary-search-tree-value/) <a id="closest-binary-search-tree-value"></a>

#### [Closest Binary Search Tree Value II](https://leetcode.com/problems/closest-binary-search-tree-value-ii/) <a id="closest-binary-search-tree-value-ii"></a>

