# Tree, 创建 / 序列化（待续）

## [Construct Binary Tree from Preorder and Inorder Traversal](https://leetcode.com/problems/construct-binary-tree-from-preorder-and-inorder-traversal/)

这道题目很综合，应用到了很多知识点，而且对树的遍历的内容也有很综合的考察

```text
             [1] 
           /     \ 
         [5]      [4]
        /   \       \ 
      [2]   [3]      [6] 
                    /
                  [7]
                  
```

* In-order: 【\(2,5,3\) 1 \(4,7,6\)】
* Pre-order: 【1 \(5,2,3\) \(4,6,7\)】
* Post-order: 【\(2,3,5\) \(7,6,4\) 1】

In-order 和 pre-order 单独都只能提供一部分树的信息，只依靠一个无法建立出完全一样的树，因为有歧义。

> * **In-order : 对于指定位置 index 的 root**
>   * **对于每个 tree / subtree, array 结构**
>   * **【左子树】【root】【右子树】**
> * **Pre-order:**
>   * **对于每个 tree / subtree, array 结构**
>   * **【root】【左子树】【右子树】**
> * **Post-order:**
>   * **对于每个 tree / subtree, array 结构**
>   * **【左子树】【右子树】【root】**

递归建树的过程是

* 建当前 root;
* 建 左/右 子树；
* 建 右/左 子树；

因此根据 inorder 和 preorder 的性质，我们用 preorder 的顺序决定“先建哪个为 root”，用 inorder 的相对位置决定 “左右子树是谁”。

因此这个问题是关于 preorder 的遍历，对于每个元素要在 inorder 中寻找相对位置。

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
    public TreeNode buildTree(int[] p, int[] in) {
        return build(p, in, 0, 0, in.length-1);
    }
    
    private TreeNode build(int[] p, int[] in, int pIndex, int inStart, int inEnd){
        if(pIndex>p.length-1||inStart>inEnd) return null;
        
        int rootNum = p[pIndex];
        int inIndex = 0;
        for(int i=0;i<p.length;i++){
            if(in[i]==rootNum)
                inIndex = i;
        }
        
        TreeNode root = new TreeNode(rootNum);
        root.left = build(p, in, pIndex+1, inStart, inIndex-1);
        root.right = build(p, in, pIndex+inIndex-inStart+1,inIndex+1, inEnd);
        
        return root;
    }
}
```

## [Construct Binary Tree from Inorder and Postorder Traversal](https://leetcode.com/problems/construct-binary-tree-from-inorder-and-postorder-traversal/)

掌握了这个性质之后，把 preorder 换成 postorder 也是一样的配方，一样的味道。

## [Serialize and Deserialize Binary Tree](https://leetcode.com/problems/serialize-and-deserialize-binary-tree/)\(待续\)

 这道题应该是LeetCode 271的姊妹题。

对于 List of String 来讲，只需要正确做好单词的分隔还有分隔符的消除歧义；而 Tree 上可以有 level 的分隔，left / right subtree 的分隔。

相对来讲Tree的优势是，这个 TreeNode 里面只存数字，所以有很多额外的字母和符号可以利用，不用担心所用字符的歧义问题（不然就得定义 escape 符了）

对于构造一个树的情况包括：[http://www.geeksforgeeks.org/serialize-deserialize-binary-tree/](http://www.geeksforgeeks.org/serialize-deserialize-binary-tree/
)

* **If given Tree is Binary Search Tree?**

  If the given Binary Tree is Binary Search Tree, we can store it by either storing preorder or postorder traversal. In case of Binary Search Trees, only preorder or postorder traversal is sufficient to store structure information.

* **If given Binary Tree is Complete Tree?**

  A Binary Tree is complete if all levels are completely filled except possibly the last level and all nodes of last level are as left as possible \(Binary Heaps are complete Binary Tree\). For a complete Binary Tree, level order traversal is sufficient to store the tree. We know that the first node is root, next two nodes are nodes of next level, next four nodes are nodes of 2nd level and so on.

* **If given Binary Tree is Full Tree?**

  A full Binary is a Binary Tree where every node has either 0 or 2 children. It is easy to serialize such trees as every internal node has 2 children. We can simply store preorder traversal and store a bit with every node to indicate whether the node is an internal node or a leaf node.

* **How to store a general Binary Tree?**

  A simple solution is to store both Inorder and Preorder traversals. This solution requires requires space twice the size of Binary Tree. We can save space by storing Preorder traversal and a marker for NULL pointers.

而这道题目给的是General Binary Tree  
**解决这题的关键就是，自己补位，构建一个 full binary tree 出来。**

第一次是因为之前construct string from binary tree的影响，用括号来标注左右子树

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
public class Codec {
    
    // Encodes a tree to a single string.
    public String serialize(TreeNode root) {
        if(root ==null) return "";
        
        String res = root.val+"";
        //if(root.left==null&&root.right==null) return res;
        String left = serialize(root.left);
        String right = serialize(root.right);
        return res+"("+left+")"+"("+right+")";
    }

    // Decodes your encoded data to tree.
    public TreeNode deserialize(String data) {
        if(data == null || data.length() == 0) return null;
        if(data.equals("()")) return null;
        
        int IndexLeft = data.indexOf('(');
        TreeNode root = new TreeNode(Integer.parseInt(data.substring(0, IndexLeft)));
        
        int count = 1;
        int IndexRight = IndexLeft+1;
        
        while(IndexRight<data.length()&&count!=0){
            if(data.charAt(IndexRight)=='(') count++;
            else if(data.charAt(IndexRight)==')') count--;
            
            IndexRight++;
        }
        
        root.left = deserialize(data.substring(IndexLeft+1, IndexRight-1));
        root.right = deserialize(data.substring(IndexRight+1, data.length()-1));
        
        return root;
    }
}

// Your Codec object will be instantiated and called as such:
// Codec codec = new Codec();
// codec.deserialize(codec.serialize(root));
```

