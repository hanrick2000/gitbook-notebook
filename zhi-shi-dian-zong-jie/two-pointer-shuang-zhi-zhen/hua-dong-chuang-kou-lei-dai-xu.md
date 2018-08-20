# 滑动窗口类 （待续）

如果涉及记录个数的或者记录元素位置的，可以使用int\[\]来记录，而int\[\]长度的选择可以参照。

* `int[26]` for Letters 'a' - 'z' or 'A' - 'Z'
* `int[128]` for ASCII
* `int[256]` for Extended ASCII

滑动窗口类的题目基本都可以用一个模板来解决。可以参看下面这一题目。

## [Minimum Size Subarray Sum](https://leetcode.com/problems/minimum-size-subarray-sum/)

1. 一个技巧是最后的return语句，可以用来判断res是否根本没有更新，进而返回正确的值（**一般使用在利用Math.max /min来更新结果的题目中**）
2. 最好使用for，并且同时初始左右两个指针，但是这样，right指针只有z在循环结束以后才会更新。同时，left更新时，无法避免减去以后，依然大于s，所以可以在减去之前，就做比较。

```java
class Solution {
    public int minSubArrayLen(int s, int[] nums) {
        int res = Integer.MAX_VALUE;
        int sum = 0;
        for(int left=0, right = 0;right<nums.length;right++){
            sum += nums[right];
            while(sum>=s){
                res = Math.min(res, right-left+1);
                sum-=nums[left++];
            
            }
        }
        return res==Integer.MAX_VALUE?0:res;
    }
}
```

## [567. Permutation in String](https://leetcode.com/problems/permutation-in-string/description/)

这道题利用了int\[\]来记录当前元素的个数。

其最终目的时保证最后count中的value都是0，

```java
class Solution {
    public boolean checkInclusion(String s1, String s2) {
        char[] arr1 = s1.toCharArray();
        int len1 = arr1.length;
        char[] arr2 = s2.toCharArray();
        int len2 = arr2.length;
        if(len1>len2) return false;
        
        int[] count = new int[128];
        for(char c : arr1)
            count[c]--;
        
        for(int l = 0, r = 0;r<len2;r++){
            if(++count[arr2[r]]>0){
                while(--count[arr2[l++]]!=0){}
            }else if(r-l+1==len1){
                return true;
            }
        }
    return len1 == 0;
    }

```

上面的做法其实技巧性比较强，针对这种permutation的题目（类似的还有anagrams的题目），都是用数组记录个数，最后整个数组为0，那其实下面这种做法也不错，特别是**AllZero函数** （java没有判断数组的值是否为某一个值得方法）

```java
class Solution {
    public boolean checkInclusion(String s1, String s2) {
        char[] arr1 = s1.toCharArray();
        int len1 = arr1.length;
        char[] arr2 = s2.toCharArray();
        int len2 = arr2.length;
        if(len1>len2) return false;
        
        int[] count = new int[128];
        
       for(int i=0;i<len1;i++){
           count[arr1[i]]++;
           count[arr2[i]]--;
       }
        if(AllZero(count)) return true;
        
        for(int i=len1;i<len2;i++){
            count[arr2[i]]--;
            count[arr2[i-len1]]++;
            if(AllZero(count)) return true;
        }
        return false;
    }
    
    private boolean AllZero(int[] a){
        for(int num:a)
            if(num!=0)
                return false;
        return true;
    }
}
```

## [3. Longest Substring Without Repeating Characters](https://leetcode.com/problems/longest-substring-without-repeating-characters/description/)

这道题目是利用int\[\]来记录元素位置的，再用这个位置去跟新左边的pointer。

```java
class Solution {
    public int lengthOfLongestSubstring(String s) {
        int len = s.length();
        if(len<=1) return len;
        int[] count = new int[128];
        Arrays.fill(count, -1);
        int res = 0;
        for(int l=0,r=0;r<len;r++){
            char temp = s.charAt(r);
            if(count[temp]!=-1){
                l = Math.max(l, count[temp]+1);
                
            }
            count[temp] = r;
            res = Math.max(res, r-l+1);
            
        }
        return res;
    }
}
```



