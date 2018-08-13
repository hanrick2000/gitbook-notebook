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

本质上对树进行了一个preorder的遍历——

root会比其子树先建立起来

对于同一个root的子树，他的右子树一定会比左子树后建立起来。

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

其实上述做法并不是这个题目最好的解法，但是我要学习这种在循环中分解一个大问题的感觉

```java
/**
 * Definition for binary tree with next pointer.
 * public class TreeLinkNode {
 *     int val;
 *     TreeLinkNode left, right, next;
 *     TreeLinkNode(int x) { val = x; }
 * }
 */
public class Solution {
    public void connect(TreeLinkNode root) {
        if(root==null) return;
        if(root.left==null&&root.right==null) return;
        
        helper(root.left, root.right);
    }
    
    private void helper(TreeLinkNode p, TreeLinkNode q){
        if(p==null&&q==null) return;
        
        p.next = q; 
        
        TreeLinkNode l = p;
        TreeLinkNode r = q;
        
        while(p.right!=null&&q.left!=null){
            p=p.right;
            q=q.left;
            
            p.next =  q;
        }
        helper(l.left, l.right);
        helper(r.left, r.right);
    }
}
```

#### 

## [117. Populating Next Right Pointers in Each Node II](https://leetcode.com/problems/populating-next-right-pointers-in-each-node-ii/description/)

这题的迭代思路和上一题一样，充分利用好上一层已经是连好的next指针来做 level order. 然而有几个很烦的地方需要处理:

* 中间有空缺节点的话，怎么让上层找到“下一个”正确的节点
* 如果某一层最左边有空缺的话，如何在进入下一层的时候找到正确的起点

这个解法的思路非常简洁优美。因为对题本质的理解更进一步了：

* **我们要做的其实是 level order 建一个 LinkedList 出来。**

于是这题几个最烦人的细节处理，都可以靠 dummy node 解决。

* **这题也是继 word ladder 之后另一个不同的 for 循环写法，字符和链表都很适合用 for loop 实现遍历。**

```java
public class Solution {
    public void connect(TreeLinkNode root) {
        while(root != null){
            TreeLinkNode dummy = new TreeLinkNode(0);
            TreeLinkNode cur = dummy;
            for( ; root != null; root = root.next){
                if(root.left != null){
                    cur.next = root.left;
                    cur = cur.next;
                }
                if(root.right != null){
                    cur.next = root.right;
                    cur = cur.next;
                }
            }
            root = dummy.next;
        }
    }
}
```

## [Binary Tree Right Side View](https://leetcode.com/problems/binary-tree-right-side-view/)

这题本质上还是在做 level order 遍历，因为最终结果上 node 是按层数依次添加的。

于是回想下递归版的 level order traversal，我们做 dfs pre-order

* 【中，左，右】 的顺序可以保证上面一层的节点永远会比下一层的先建立；
* 同时对于每一层，我们一定会先发现最左面的节点，而后才是右面依次发现。

把这个性质根据题意改一下，我们就有了

* **【中，右，左】 首先保证了 level order，因为对于所有子树，中节点要比左右子树先执行；**
* **同时因为子树顺序是 【右，左】，同一 level 的节点一定是从右到左被发现的。**

因此只要注意判断条件，只把第一次发现的元素添加进去，然后做改变顺序版 preorder traversal 就好了。

* **同理也很容易解 Left Side View.**

```java
public class Solution {
    public List<Integer> rightSideView(TreeNode root) {
        List<Integer> list = new ArrayList<Integer>();

        helper(root, list, 0);

        return list;
    }

    private void helper(TreeNode root, List<Integer> list, int level){
        if(root == null) return;

        if(level == list.size()) list.add(root.val);
        helper(root.right, list, level + 1);
        helper(root.left, list, level + 1);
    }
}
```

## [101. Symmetric Tree](https://leetcode.com/problems/symmetric-tree/description/)

这道题目其实不是用level order来做的。

但是我第一次拿到这个题目的时候，就想从看这个tree的每一个level是否是对称的（即层次遍历后，得到的是否是回文来入手）

但是这个题目与上面的 **Populating Next Right Pointers in Each Node**不一样：它不是完美二叉树，**从而导致很多奇怪的拐角**。所以它并不适合用层次遍历来做

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



