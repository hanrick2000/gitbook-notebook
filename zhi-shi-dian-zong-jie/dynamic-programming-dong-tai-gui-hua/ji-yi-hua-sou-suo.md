# 记忆化搜索 （待续）

[http://happylcj.github.io/2015/08/04/%E8%AE%B0%E5%BF%86%E5%8C%96%E6%90%9C%E7%B4%A2%E4%B8%93%E9%A2%98%E8%AE%AD%E7%BB%83/](http://happylcj.github.io/2015/08/04/%E8%AE%B0%E5%BF%86%E5%8C%96%E6%90%9C%E7%B4%A2%E4%B8%93%E9%A2%98%E8%AE%AD%E7%BB%83/)

## 记忆化搜索 VS DP

记忆化搜索算法上依然是搜索的流程，但是搜索到的一些解用动态规划的那种思想和模式作一些保存；一般说来，动态规划总要遍历所有的状态，而搜索可以排除一些无效状态。更重要的是搜索还可以剪枝，**可能剪去大量不必要的状态，因此在空间开销上往往比动态规划要低很多**。

  
记忆化算法在求解的时候还是按着自顶向下的顺序，但是每求解一个状态，就将它的解保存下来，**以后再次遇到这个状态的时候，就不必重新求解了**。

  
**这种方法综合了搜索和动态规划两方面的优点，因而还是很有实用价值的。可以归纳为：记忆化搜索=搜索的形式+动态规划的思想**

### **记忆化搜索是自顶向下，SP是自底向上。**

但是你可以想下：

从上往下的搜索方式是不是真的会计算所有节点？ 还是会省略一些不必要的节点？**会省略一些不必要的节点。**

如果记忆化搜索会展开所有节点铺满一个我们的矩阵，那是不是我们直接自底向上更好？**是的**

记忆化搜索可能需要保存一个巨大的记录矩阵，但是动态规划有时可以采用滚动数组来减掉一维或更多，空间有优势。  


二者其实差不多，先想到什么就用什么

## [Longest Increasing Path in a Matrix](https://leetcode.com/problems/longest-increasing-path-in-a-matrix/)\*\*\*\*

去年的 Google 高频题，著名的 POJ 1088 - 滑雪。

**这题是 2D-array 上的搜索问题，参考 number of islands 和 word search.**

* **Optimal Substructure**
  * **全局最优解一定由从起点 maxPath\(i , j\) 和其相邻的有效起点 maxPath\(i', j'\) 的 subproblem 构造而成**
  * **由于最优解 path 对单调性有要求，因此无需修改原矩阵，也无需担心下一个位置回头造成死循环**
* **Overlap Subproblems**
  * **对于每一个起点 maxPath\(i , j\) ，都需要 top-bottom 的递归解决其下一个有效位置 maxPath\(i', j'\) 的子问题，并且高度覆盖。**

```java
class Solution {
    int row = 0, col = 0;
    public int longestIncreasingPath(int[][] matrix) {
        if(matrix==null||matrix.length==0) return 0;
        row = matrix.length;
        col = matrix[0].length;
        int[][] dp = new int[row][col];
        
        int max = 0;
        for(int i=0;i<row;i++){
            for(int j=0;j<col;j++){
                max = Math.max(max, memorizedSearch(matrix, i, j, dp));
            }
        }
        return max;
    }
    
    private int memorizedSearch(int[][] matrix, int i, int j, int[][] dp){
        if(i<0||i>=row) return 0;
        if(j<0||j>=col) return 0;
        
        if(dp[i][j]!=0) return dp[i][j];
        
        if(i-1>=0&&matrix[i-1][j]>matrix[i][j]) dp[i][j] = Math.max(dp[i][j], memorizedSearch(matrix, i-1, j, dp));
        if(i+1<row&&matrix[i+1][j]>matrix[i][j]) dp[i][j] = Math.max(dp[i][j], memorizedSearch(matrix, i+1, j, dp));
        if(j-1>=0&&matrix[i][j-1]>matrix[i][j]) dp[i][j] = Math.max(dp[i][j], memorizedSearch(matrix, i, j-1, dp));
        if(j+1<col&&matrix[i][j+1]>matrix[i][j]) dp[i][j] = Math.max(dp[i][j], memorizedSearch(matrix, i, j+1, dp));
        
        
        return ++dp[i][j];
    }
}
```

