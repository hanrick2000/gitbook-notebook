# Array的边界值

## [696. Count Binary Substrings](https://leetcode.com/problems/count-binary-substrings/description/)

对于字符串的处理，我还是习惯转换成char Array。

对于这样的char array，处理他们的边界值，就使用arr\[i\]==arr\[i-1\]，如果使用arr\[i\]==arr\[i+1\]，处理边界值会非常麻烦。

```java
class Solution {
    public int countBinarySubstrings(String s) {
        char[] arr = s.toCharArray();
        int count1 = 0, count2 = 1, res =0;
        for(int i=1;i<arr.length;i++){
            if(arr[i]==arr[i-1]){
                count2++;
            }else{
                res += Math.min(count1, count2);
                count1 = count2;
                count2 = 1;
            }
            
        }
        res += Math.min(count1, count2);
        return res;
    }
}
```

