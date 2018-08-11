---
description: 对撞型指针的本质是，不需要扫描多余的状态。在两边指针的判断之后，可以直接跳过其中一个指针与所有另外一面 index 组成的 pair.
---

# 对撞型，water/sum类

## [Triangle Count](http://www.lintcode.com/en/problem/triangle-count/)

1. 计算三角形的时候，只需要满足两边（较小边）之和大于第三边就好了
2. 使用对撞类的双指针的时候，基本是利用了一种单调性，一次只把一个指针向一个方向推，而且不用回头
3. 所以顺着这个问题想，如果我们最外围遍历的是 k，而 i, j 都处于 k 的左面，那么每次就只有一个正确方向，挪动一个指针可以保证正确性了，即让 num\[i\] + num\[j\] 变大，或者变小。

```java
public class Solution {
    /**
     * @param S: A list of integers
     * @return: An integer
     */
    public int triangleCount(int[] S) {
        // write your code here
        Arrays.sort(S);
        int len = S.length;
        if(len<=2) return 0;
        int res = 0;
        for(int k=2;k<len;k++){
            int i=0;
            int j=k-1;
            
            while(i<j){
                if(S[i]+S[j]<=S[k])
                    i++;
                else{
                    res+=j-i;
                    j--;
                }
                    
            }
        }
        return res;
    }
}
```

## [Container With Most Water](http://www.lintcode.com/en/problem/container-with-most-water/)

这种“灌水”类问题和双指针脱不开关系，都要根据木桶原理维护边界；在矩阵中是外围的一圈高度，在数组中就是左右两边的高度。

Initially we consider the area constituting the exterior most lines. Now, to maximize the area, we need to consider the area between the lines of larger lengths. If we try to move the pointer at the longer line inwards, we won't gain any increase in area, since it is limited by the shorter line. But moving the shorter line's pointer could turn out to be beneficial, as per the same argument, despite the reduction in the width. This is done since a relatively longer line obtained by moving the shorter line's pointer might overcome the reduction in area caused by the width reduction.

For further clarification click [here](https://discuss.leetcode.com/topic/3462/yet-another-way-to-see-what-happens-in-the-o-n-algorithm) and for the proof click [here](https://discuss.leetcode.com/topic/503/anyone-who-has-a-o-n-algorithm/2).

简单翻译就是，如果移动较长的木板，那面积肯定会减小，所以移动较短的木板，那面积就会上涨

```java
class Solution {
    public int maxArea(int[] height) {
        int left = 0, right = height.length-1;
        //int leftMax = 0, leftRight = 0; 
        int res = 0;
        int max = 0;
        while(left<right){
            max = Math.max(max, Math.min(height[left], height[right])*(right-left));
            if(height[left]<height[right])
                left++;
            else
                right--;
        }
        return max;
    }
}
```

## Trapping Rain Water



木桶原理的算法版，最两边的板子形成一个木桶，由两块板子最低的那块决定水位。每次都从两边最低的方向向里扫描。

* **因为 i,j 当前高度未必是当前左右两边最高高度，每次更新的时候要注意维护下。**



