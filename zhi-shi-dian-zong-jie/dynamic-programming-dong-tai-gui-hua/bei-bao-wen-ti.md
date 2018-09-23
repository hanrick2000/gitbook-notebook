# 背包问题

## 特点

1. 至少要用**目标值**作为一个DP的维度
2. 对具体元素敏感（比如01背包），但是对同意total size/value 来讲，有时这些total如何组成（构成的具体线路）有时并不那么重要。**这是由元素特性和背包 size 的约束条件决定的，也是有别于其他种类 DP 的一个重要区别。（这里的背包size就是特点1中提到的目标值）比如 Combination Sum IV，就是典型的 “对最终结果敏感，对元素个数不敏感” 的问题，每次可以取任意元素，任意个，因此只用 dp\[sum\] 一个维度做 dp 就够了。**
3. 在只取 min/max 的时候我们不同的循环顺序都能保证正确答案；但是求解的数量时，要以 target value 为外循环。同时，当list里面的值可以重复使用的时候。也应该吧对list的循环放到内循环来。否则，还是优先选择将对list的遍历放在外循环。

## 总结

背包类DP，重点在于：元素的具体位置（前几个元素）和数量（元素的总个数是多少）是否对结果产生影响。

**因此可以看到，只有对元素的选择\(位置\) / \(数量\)有限制性条件的 DP，才会需要另一个维度。**

* **均没有要求的：Backpack，Backpack III，coin change， cutting a rope。通常是元素可以无限次取。用一个一维数组维护target就好了。**
* **对位置有要求的（通常意味着某个元素只能取一次），用 i 代表“前 i 个元素”；**
* **对数量有要求的，用 j 代表“选 k 个元素”；**
* **两个全有要求的，就两个维度都加上去。**
* **两个维度都加上去，我们就有了 k sum 问题。**

对于判断是否为真的做法，将target放到内循环，并且从最大值开始向下遍历，否则，容易将整个DP队列都变成一个值

## [Backpack](http://www.lintcode.com/en/problem/backpack/)

先从暴力搜索开始，结构图如下：

![](https://mnmunknown.gitbooks.io/algorithm-notes/content/backpack.jpg)

对于每个元素都会衍生出 \(取/不取\) 两种选择，所有可能的策略则是从最左面到最右面的一条 path，也即 binary tree 从 root 到 leaf node 的路径个数。

**显然，路径个数 = 叶节点个数 = 2^n.**

接下来的观察是关于题目性质的，可以发现我们最关注的是 “sum”，而不太关心这个 sum 是怎么来的。比如 sum = 10 的时候，我们可以取【2,3,5】 或者 【3,7】，虽然是两种不同的解，但是最终效果完全一样，也就是完全一样的“状态”。因此，如果我们用 sum 来定义状态，就会发现很多的 overlap subproblems，可以进一步采用记忆化搜索进行优化。

另一个需要注意的地方是，每个元素只能取一次，只有 “取” 或者 “不取” 的选择，因此当前 index 上的状态只能取决于之前的状态，而不能重复考虑当前元素。

考虑到“位置敏感”，所以采用二维，同时数组的update都是通过前一个“位置”来的。所以在位置这个维度，可以使用滚动数组，利用取mod的做法，只保留二维

```java
public class Solution {
    /**
     * @param m: An integer m denotes the size of a backpack
     * @param A: Given n items with size A[i]
     * @return: The maximum size
     */
    public int backPack(int m, int[] A) {
        // write your code here
        boolean[][] dp = new boolean[m+1][2];
        
        for(int i = 0; i <= A.length; i++) dp[0][i % 2] = true;
        
        
            for(int j = 1; j<=A.length; j++) {
                for(int i=1;i<=m;i++) {
                if(i>=A[j-1]) {
                    dp[i][j%2] = dp[i][(j-1)%2] || dp[i-A[j-1]][(j-1)%2];
                    
                } else {
                    dp[i][j%2] = dp[i][(j-1)%2];
                }
            }
        }
        for(int i=m;i>=0;i--) {
            if(dp[i][A.length%2])
                return i;
        }
        return -1;
    }
}
```

Backpack也可以只维护一个维度，原因是虽然这个问题对位置敏感，但是他并不要求求出实际的个数，所以可以只维护一个bool型的数组。

```java
public class Solution {
    /**
     * @param m: An integer m denotes the size of a backpack
     * @param A: Given n items with size A[i]
     * @return: The maximum size
     */
    public int backPack(int m, int[] A) {
        // write your code here
        boolean[] dp = new boolean[m+1];
        dp[0] = true;
        
        for(int a : A) {
            for(int i=m;i>=0;i--) {
                if(i>=a) {
                    dp[i] = dp[i] || dp[i-a];
                }
            }
        }
        
        for(int i=m;i>=0;i--) {
            if(dp[i]) 
                return i;
        }
        return -1;
    }
}
```

## [Backpack II](http://www.lintcode.com/en/problem/backpack-ii/)

这次每个元素除了 size 之外也具有 value，就变成了更典型的 01 背包问题。

问题的结构依然具有 01 背包的性质，选一条 path 使得最终总价值最大，path 总数为 2^n. 同时对于同一个总的空间占用 totalSize 或者 totalValue，我们并不在乎它是怎么构造出来的，只要不重复选元素就行。

因此我们 dp 结构一致，而采用 int\[\]\[\] 来记录

**dp\[i\]\[j\] 包里有 j 的空间，可以取前 i 个元素时，所能获得的最大收益。**

**同时因为每次新迭代循环中， i 是不会走回头路的，所以状态与状态之间可以在不违反题意的情况下衔接起来。**

```text
public class Solution {
    /**
     * @param m: An integer m denotes the size of a backpack
     * @param A & V: Given n items with size A[i] and value V[i]
     * @return: The maximum value
     */
    public int backPackII(int m, int[] A, int V[]) {
        // write your code here
        // dp[i][j] within first i elements with space j, maximum profit gain
        int[][] dp = new int[A.length + 1][m + 1];

        for(int i = 1; i <= A.length; i++){
            for(int j = 1; j <= m; j++){
                if(j - A[i - 1] >= 0){
                    dp[i][j] = Math.max(dp[i - 1][j] , dp[i - 1][j - A[i - 1]] + V[i - 1]);
                } else {
                    dp[i][j] = dp[i - 1][j];
                }
            }
        }

        return dp[A.length][m];
    }
}
```

## [Cutting a Rod](https://www.lintcode.com/problem/cutting-a-rod/description)

这道题目和Backpack III 非常相似，即，同一数值可以取任意多次。所以这里只用维护一个维度了。

```java
public class Solution {
    /**
     * @param prices: the prices
     * @param n: the length of rod
     * @return: the max value
     */
    public int cutting(int[] prices, int n) {
        // Write your code here
        int[] dp = new int[n+1];
        dp[0] = 0;
        for(int i=0;i<=n;i++) {
            for(int j=0;j<prices.length;j++) {
                if(i>=j+1) {
                    dp[i] = Math.max(dp[i], dp[i-j-1] + prices[j]);
                }
            }
        }
        return dp[n];
    }
}

```

## \(G\) [Minimal Partition](https://www.lintcode.com/problem/minimum-partition/description) 

这个问题基本可以抽象为backpack I

```java
public class Solution {
    /**
     * @param nums: the given array
     * @return: the minimum difference between their sums 
     */
    public int findMin(int[] nums) {
        // write your code here
        //int len = nums.length;
        int sum = 0;
        for(int a : nums) {
            sum += a;
        }
        
        int mid = sum / 2;
        boolean[] dp = new boolean[mid+1];
        dp[0] = true;
        
        for(int a : nums) {
            for(int i=mid;i>=0;i--) {
                if(i>=a) {
                    dp[i] |= dp[i-a];
                } 
            }
        }
        
        for(int i = mid; i>=0; i--) {
            if(dp[i]) 
                return sum - i - i;
        }
        return -1;
    }
}


```



## [Combination Sum IV](https://leetcode.com/problems/combination-sum-iv/)

原理和 climbing stairs 差不多，建一个大小等于 target + 1 的 array 代表多少种不同的方式跳上来，依次遍历即可。

时间复杂度 O\(n \* target\)

**注意这里内外循环的顺序不能错，要先按 sum 从小到大的顺序看，然后才是遍历每个元素。因为所谓 bottom-up，是相对于 sum 而言的，不是相对于已有元素的 index 而言的。**

**可以看到，在只取 min/max 的时候我们不同的循环顺序都能保证正确答案；但是求解的数量时，要以 target value 为外循环。**

```text
public class Solution {
    public int combinationSum4(int[] nums, int target) {
        // dp[sum] = number of ways to get sum 
        int[] dp = new int[target + 1];

        // initialize, one way to get 0 sum with 0 coins
        dp[0] = 1;

        for(int j = 1; j <= target; j++){
            for(int i = 0; i < nums.length; i++){
                if(j - nums[i] >= 0) dp[j] += dp[j - nums[i]]; 
            }
        }

        return dp[target];
    }
}
```

## [K sum](http://www.lintcode.com/en/problem/k-sum/)

背包类问题的扩展版本~

**我们关注的维度有三个：**

* **前 i 个元素，因为一个元素只能选一次；**
* **选了 j 个元素，因为我们要求选 k 个数；**
* **总和 sum，用于每次试图添加新元素时用来查询。**

**时间：O\(len \* k \* target\)，三重循环**

**空间：O\(k \* target\)**

```text
public class Solution {
    /**
     * @param A: an integer array.
     * @param k: a positive integer (k <= length(A))
     * @param target: a integer
     * @return an integer
     */
    public int kSum(int A[], int k, int target) {
        // int[i][j][k] : number of ways to get sum of k by picking
        //                j numbers from first i numbers
        int[][][] dp = new int[2][k + 1][target + 1];

        dp[0][0][0] = 1;
        dp[1][0][0] = 1;

        for(int i = 1; i <= A.length; i++){
            for(int j = 1; j <= k; j++){
                for(int v = 1; v <= target; v++){
                    // i has an offset of 1, becasue when i = 1
                    // we are actually referring to index 0
                    if(v >= A[i - 1]){
                        dp[i % 2][j][v] = dp[(i - 1) % 2][j][v] + 
                                          dp[(i - 1) % 2][j - 1][v - A[i - 1]];
                    } else {
                        dp[i % 2][j][v] = dp[(i - 1) % 2][j][v];
                    }
                }
            }
        }
        return dp[A.length % 2][k][target];
    }
}
```



