# Missing Number

##  [Move Zeros](https://leetcode.com/problems/move-zeroes/)

FB 的超高频，记得先演一演，给个 swap 的写法~

这种写法中，我们调用 swap 的次数取决于从数组第一个 0 开始，后面到底有多少个非 0 元素。

**一次 swap = 2次数组写入 + 1次 temp variable 写入，总写入次数为 O\(3n\)，总数组写入次数为 O\(2n\).**

```text
public class Solution {
    public void moveZeroes(int[] nums) {
        if(nums == null || nums.length == 0) return;
        int ptr = 0;
        while(ptr < nums.length && nums[ptr] != 0) ptr ++;
        for(int i = ptr + 1; i < nums.length; i++){
            if(nums[i] != 0) swap(nums, i, ptr++);
        }
    }

    private void swap(int[] nums, int a, int b){
        int temp = nums[a];
        nums[a] = nums[b];
        nums[b] = temp;
    }
}
```

然后 follow-up 肯定是如何减少写入次数 \(aka 别用 swap\)，后面的演技就是这样……

其实我一直觉得这个写法才是最直观的，暴力点用 swap 的写法反而 tricky 一点。。

**这种写法中，数组每一个位置一定会被不多不少写入一次，写入次数为 O\(n\)**

```java
    public void moveZeroes(int[] nums) {
        if(nums == null || nums.length == 0) return;
        int ptr = 0;
        for(int i = 0; i < nums.length; i++){
            if(nums[i] != 0) nums[ptr++] = nums[i];
        }
        for(; ptr < nums.length; ptr++){
            nums[ptr] = 0;
        }
    }
```

## [Missing Number](https://leetcode.com/problems/missing-number/)

等差数列。 英文叫 **arithmetic sequence**，记一下，和老外面试官吹比用。。

```java
public class Solution {
    public int missingNumber(int[] nums) {
        int N = nums.length;
        int sum = 0;
        for(int num : nums) sum += num;
        return  (N + 1) * N / 2 - sum;
    }
}
```



