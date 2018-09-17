# DFS flood filling（待续）

DFS filling其实也就是DFS+染色的一种思想

## [733. Flood Fill](https://leetcode.com/problems/flood-fill/description/)

这道题目简直就是这个专题的标志例题，其实做法很简单

就是注意Array是mutable的变量，可以在各个recursive中传递值。

而且，void类的函数，既可以直接return，也可以什么都不返回

```java
class Solution {
    public int[][] floodFill(int[][] image, int sr, int sc, int newColor) {
        int ori = image[sr][sc];
        if(ori!=newColor)dfs(image, sr, sc, newColor, ori);
        return image;
    }
    
    private void dfs(int[][] image, int x, int y, int newColor, int ori){
        if(x<0||x>=image.length) return;
        if(y<0||y>=image[0].length) return;
        
        if(image[x][y]==ori){
            image[x][y] = newColor;
            dfs(image, x-1, y, newColor, ori);
            dfs(image, x+1, y, newColor, ori);
            dfs(image, x, y-1, newColor, ori);
            dfs(image, x, y+1, newColor, ori);
        }else
            return;
    }
}
```

