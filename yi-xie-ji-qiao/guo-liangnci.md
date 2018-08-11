---
description: 重复N次类似的操作，时间复杂度不会增加，适用于前后的数据对答案都有影响的情况
---

# 过两（N）次

## 正反遍历数据两次

### [542. 01 Matrix](https://leetcode.com/problems/01-matrix/description/)

```java
class Solution {
    public int[][] updateMatrix(int[][] matrix) {
        if(matrix==null||matrix.length==0) return null;
        
        
        int len = matrix.length, hei = matrix[0].length;
        int[][] dis = new int[len][hei];
        int range = len*hei;
        
        for(int i=0;i<len;i++){
            for(int j=0;j<hei;j++){
                if(matrix[i][j]==0){
                    dis[i][j] = 0;
                }else{
                    int left = i==0?range:dis[i-1][j];
                    int up = j==0?range:dis[i][j-1];
                    dis[i][j] = Math.min(left, up)+1;
                }
            }
        }
        
        for(int i=len-1;i>=0;i--){
            for(int j=hei-1;j>=0;j--){
                int right = i>=len-1?range:dis[i+1][j];
                int down = j>=hei-1?range:dis[i][j+1];
                dis[i][j] = Math.min(dis[i][j], Math.min(right, down)+1);
            }
        }
        
        return dis;
    }
}
```

### [Product of Array Except Self](https://leetcode.com/problems/product-of-array-except-self/description/)

Apple Siri OA

```java
class Solution {
    public int[] productExceptSelf(int[] nums) {
        int len  = nums.length;
        
        int[] res = new int[len];
        res[0] = 1;
        for(int i=1;i<len;i++){
            res[i] = res[i-1]*nums[i-1];
        }
        int product = 1;
        for(int i=len-1;i>=0;i--){
            res[i]*=product;
            product*=nums[i];
        }
        
        return res;
    }
}

```

### [135. Candy](https://leetcode.com/problems/candy/description/)

这里的2-pass，是因为前后neighbor会对当前的数有影响

```java
class Solution {
    public int candy(int[] ratings) {
        int len = ratings.length;
        int[] candy = new int[len];
        
        Arrays.fill(candy, 1);
        for(int i=1;i<len;i++){
            if(ratings[i]>ratings[i-1])
                candy[i] = candy[i-1]+1;
        }
        
        int res = 0;
        for(int i=len-1;i>0;i--){
            if(ratings[i]<ratings[i-1])
                candy[i-1] = Math.max(candy[i-1], candy[i]+1);
            res+=candy[i];
        }
        return res+candy[0];
    }
}
```



