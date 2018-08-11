# Subarray

Subarray类的题目一般是问一个Array里面符合某个条件的Subarray的长度Or个数Or直接Print

一般就是采用HashMap的方法，存储位置，个数等信息。

感觉与Substring类的问题挺类似的。

## [525. Contiguous Array](https://leetcode.com/problems/contiguous-array/description/)

就是按照上面说的那种思路。注意题目要求：一般问的限制条件是什么，hashMap里面就放什么。

```java
class Solution {
    public int findMaxLength(int[] nums) {
        Map<Integer, Integer> map = new HashMap<>();
        int len = nums.length;
        
        int count  = 0;
        int max = 0;
        for(int i=0;i<len;i++){
            if(nums[i]==1) count++;
            else if(nums[i]==0) count--;
            if(count==0) max = Math.max(i+1, max);
            else if(map.containsKey(count)){
                int temp = i-map.get(count);
                max = Math.max(max, temp);
            }else{
                map.put(count, i);
            }
        }
        return max;
    }
}
```

## 523. Continuous Subarray Sum

这道题我错了无数次，他有许多corner case要考虑。而我最开始想要用的类似记忆化搜索的方法，特别容易超内存限制，所以还是不建议使用。

还有对于取余数的题目，结果只会与余数有关，所以每次循环的时候，都可以进行取余数的运算，之后只对余数进行考虑。

```java
class Solution {
    public boolean checkSubarraySum(int[] nums, int k) {
        
        
        Map<Integer, Integer> map = new HashMap<Integer, Integer>(){{put(0,-1);}};;
        int Sum = 0;
        for(int i=0;i<nums.length;i++){
            Sum+=nums[i];
            if(k!=0) Sum%=k;
            Integer prev = map.get(Sum);
            if(prev!=null){
                if(i-prev>1) return true; 
            }
            else map.put(Sum, i);
        }
        return false;
    }
}
```

