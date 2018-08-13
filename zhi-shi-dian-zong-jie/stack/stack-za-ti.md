# Stack杂题

## [316. Remove Duplicate Letters](https://leetcode.com/problems/remove-duplicate-letters/description/)

这道题其实算很典型的Greedy的思想：当前字符的计数不为0的时候，但凡遇到比他前面的数据，全部不算——尽量在前面放小的。 根据这种思路，可以想到使用Stack

```text
class Solution {
    public String removeDuplicateLetters(String s) {
        char[] arr = s.toCharArray();
        int[] count = new int[128];
        for(char c:arr){
            count[c]++;
        }
        
        Stack<Character> stack = new Stack<>();
        
        for(char c:arr){
            count[c]--;
            if(stack.contains(c)) continue;
            while(!stack.isEmpty()&&c<stack.peek()&&count[stack.peek()]>0)
                stack.pop();
            stack.push(c);
            
        }
        StringBuilder sb = new StringBuilder();
        while(!stack.isEmpty())
            sb.append(stack.pop());
        
        return sb.reverse().toString();
    }
}
```

