# Backtracking+DFS与Tree

## [257. Binary Tree Paths](https://leetcode.com/problems/binary-tree-paths/description/)

DFS + Backtracking 都有三个步骤：

* Add element
* DFS
* Remove element

其中随着 dfs 传递参数的不同，三个步骤的位置会有变化。

**在 Tree 上做 dfs + backtracking 比较适合用 dfs 带着当前考虑的 node 为参数，先 Add 然后在 leaf node 上做 Remove 的方式；**

**在中间结果是 String 的情况下，如果想保存一个 object 的 reference 可以用 StringBuilder, 同时也可以利用 immutable 的性质直接传新的 String copy \(空间占用多一些\)，这样可以免去回溯的步骤。**

```java
class Solution {
    /**
     * @param nums: A list of integers.
     * @return: A list of permutations.
     */
    public ArrayList<ArrayList<Integer>> permute(ArrayList<Integer> nums) {
        // write your code here
        ArrayList<ArrayList<Integer>> result = new ArrayList<ArrayList<Integer>>();
        if(nums == null || nums.size() == 0) return result;

        helper(result, new ArrayList<Integer>(), nums);
        return result;
    }

    private void helper(ArrayList<ArrayList<Integer>> result, 
                        ArrayList<Integer> list,
                        ArrayList<Integer> nums){

        if(list.size() == nums.size()){
            result.add(new ArrayList<Integer>(list));
            return;
        }          

        for(int i = 0; i < nums.size(); i++){
            if(list.contains(nums.get(i))) continue;
            list.add(nums.get(i));
            helper(result, list, nums);
            // 抹掉末尾
            list.remove(list.size() - 1);
        }
    }
}
```

```java
public class Solution {
    public List<String> binaryTreePaths(TreeNode root) {
        List<String> list = new ArrayList<String>();
        if(root == null) return list;
        dfs(list, root, new StringBuilder());
        return list;
    }

    private void dfs(List<String> list, TreeNode node, StringBuilder sb){
        if(node == null) return;

        // 存盘，记录当前位置
        int length = sb.length();
        ------------ADD-------------
        sb.append(node.val);
        ------------Check--------------
        if(node.left == null && node.right == null){
            list.add(sb.toString());
            // 归位（抹掉最后一个数字）
            sb.setLength(length);
            return;
        }

        -------------DFS---------------
        sb.append("->");
        dfs(list, node.left, sb);
        dfs(list, node.right, sb);
        -----------Backtrack-------------
        // 归位（抹掉最后一个箭头和前面的数字）
        sb.setLength(length);
    }
}
```

一个先加后 dfs 的例子，看起来稍微蛋疼了点；

```java
public class Solution {
    public List<String> binaryTreePaths(TreeNode root) {
        List<String> list = new ArrayList<String>();
        if(root == null) return list;
        StringBuilder sb = new StringBuilder();
        sb.append(root.val);
        if(root.left == null && root.right == null){
            list.add(sb.toString());
            return list;
        }
        dfs(list, root, sb);
        return list;
    }

    private void dfs(List<String> list, TreeNode node, StringBuilder sb){
        if(node == null) return;

        if(node.left == null && node.right == null){
            list.add(sb.toString());
            return;
        }

        sb.append("->");
        int length = sb.length();
        if(node.left != null){
            sb.append(node.left.val);
            dfs(list, node.left, sb);
            sb.setLength(length);
        }
        if(node.right != null){
            sb.append(node.right.val);
            dfs(list, node.right, sb);
            sb.setLength(length);
        }

    }
}
```

另一个比较有意思的解法，利用 String immutable 默认生成新 copy 的办法，当你 dfs 之间不是 by reference 指向同一个 object / collection ，也就不需要 backtracking 了。

```java
public class Solution {
    public List<String> binaryTreePaths(TreeNode root) {
        List<String> res = new ArrayList<String>();
        if (root == null){
            return res;
        }
        return getPaths (root, "", res);
    }

    public List<String> getPaths(TreeNode root, String str, List<String> res){
        if (root.left == null && root.right == null){
            if (str.equals("")){
                str += root.val;
            } else {
                str += "->"+root.val;
            }
            res.add (str);
            return res;
        }

        if (str.equals("")){
            str += root.val;
        } else {
            str += "->"+root.val;
        }

        if (root.left != null)  getPaths (root.left, str, res);
        if (root.right != null)  getPaths (root.right, str, res);

        return res;
    }
}
```

