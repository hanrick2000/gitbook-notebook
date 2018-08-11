---
description: >-
  当我们需要 in-place 处理矩阵的时候，最简便直接的思路往往是“多个 pass”的，即用一次 pass 做标记，第二次甚至第三次 pass 再做实际处理。
---

# [Spiral Matrix](https://leetcode.com/problems/spiral-matrix/)
这道题目其实就是维护四个边界，步步向内紧缩。思路很简单，但是边界值的处理过于麻烦，特别容易弄混。

特别注意处理 “下” 和 “左” 边界的时候，有可能当前这层只有一行，或者一列，已经输出过了不需要重复输出。所以这两条边的循环上要注意加一个判断条件。

```java
class Solution {
    public List<Integer> spiralOrder(int[][] matrix) {
        List<Integer> res = new ArrayList<>();
        if(matrix == null || matrix.length == 0) return res;

        int colstart = 0;
        int colend = matrix[0].length-1;
        int rowstart = 0;
        int rowend = matrix.length-1;
        
        while(colstart<=colend&&rowstart<=rowend){
            for(int i=colstart;i<=colend;i++){
                res.add(matrix[rowstart][i]);
            }
            rowstart++;
            for(int i=rowstart;i<=rowend;i++){
                res.add(matrix[i][colend]);
            }
            colend--;
            if(rowstart<=rowend)
                for(int i=colend;i>=colstart;i--)
                    res.add(matrix[rowend][i]);
            rowend--;
            if(colstart<=colend)
                for(int i=rowend;i>=rowstart;i--)
                    res.add(matrix[i][colstart]);
            colstart++;
        }
        return res;
    }
}
```
# [80. Remove Duplicates from Sorted Array II](https://leetcode.com/problems/remove-duplicates-from-sorted-array-ii/description/)
这道题目同样是in-place地处理array, 在循环中已经处理过的前面的数组，可以用后面的去覆盖。还有就是注意一下sorted这个特点，我时常会忽略sorted后的数组有多麽多骚操作
```java
class Solution {
    public int removeDuplicates(int[] nums) {
        int idx = 0;
        for(int num:nums){
            if(idx<2||num>nums[idx-2])
                nums[idx++]=num;
        }
        return idx;
    }
}
```

