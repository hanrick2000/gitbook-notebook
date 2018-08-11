# Dynamic Programming, 动态规划

这个[帖子](https://leetcode.com/articles/jump-game/?page=4)是我目前看到的把DP讲的最明白的帖子：

对于一个DP问题，通常是思维路线是一下4步：

1. Start with the recursive backtracking solution（基本的BackTracking递归）
2. Optimize by using a memoization table \(top-down\[3\] dynamic programming\) （记忆化搜索，同样采用递归的写法，从初始状态一直推到结束状态）
3. Remove the need for recursion \(bottom-up dynamic programming\) （用迭代代替了递归，从结束状态推初始状态）
4. Apply final tricks to reduce the time / memory complexity（用一些小技巧去优化当前DP）

