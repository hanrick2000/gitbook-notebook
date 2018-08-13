# circular array，环形数组

## [503. Next Greater Element II](https://leetcode.com/problems/next-greater-element-ii/discuss/98262/Typical-ways-to-solve-circular-array-problems.-Java-solution.)

### 延长数组至原来的两倍长

这样做以后也就不用再考虑回到开始的问题。

但是这种方法其实就是一种暴力的做法，只是节省了一定的取余运算的时间。所以时间复杂度依然是n^2。

可以用`System.arraycopy(nums, 0, doublenums, 0, nums.length);` 复制数组

```java
class Solution {
    public int[] nextGreaterElements(int[] nums) {
        int len = nums.length;
        int[] res = new int[len];
        int[] temp = new int[len*2];
         
        System.arraycopy(nums, 0, temp, 0, nums.length);
        System.arraycopy(nums, 0, temp, nums.length, nums.length);
        
        for(int i=0;i<len;i++){
            res[i] = -1;
            for(int j=i+1;j<2*len;j++){
                if(temp[j]>nums[i]){
                    res[i] = temp[j];
                    break;
                }
            }
        }
        return res;
    }
}
```

### 利用堆栈的方法

 This stack stores the indices of the appropriate elements from nums array. The top of the stack refers to the index of the Next Greater Element found so far. We store the indices instead of the elements since there could be duplicates in the **nums** array. 

```java
class Solution {
    public int[] nextGreaterElements(int[] nums) {
        Stack<Integer> s = new Stack<>();
        int len = nums.length;
        int[] res = new int[len];
        for(int i=len-1;i>=0;i--){
            s.push(i);
        }
        
        for(int i=len-1;i>=0;i--){
            res[i] = -1;
            while(!s.isEmpty()&&nums[i]>=nums[s.peek()])
                s.pop();
            if(!s.isEmpty())
                res[i] = nums[s.peek()];
            s.add(i);
        }
        return res;
    }
}
```



