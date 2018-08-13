# Array向两边扩散

这种方法应该是比较常用于回文串的检测（但是回文串的检测好像有更好用的通解Manarcher算法）。但是还是记录在这里，多提供一种思路。

## [647. Palindromic Substrings](https://leetcode.com/problems/palindromic-substrings/description/)

需要注意的是

**回文串的判断，一定要注意区分奇数与偶数回文串**

```java
class Solution {
    int count = 0;
    public int countSubstrings(String s) {
        char[] arr = s.toCharArray();
        for(int i=0;i<s.length();i++){
            helper(arr,i,i);
            helper(arr,i,i+1);
        }
        return count;
    }
    
    private void helper(char[] arr, int i, int j){
        while(i>=0&&j<arr.length&&arr[i]==arr[j]){
            count++;
            i--;
            j++;
        }
    }
}
```

